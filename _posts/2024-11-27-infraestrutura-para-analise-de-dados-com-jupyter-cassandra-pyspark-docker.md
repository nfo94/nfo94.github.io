---
title: "Infraestrutura para análise de dados com Jupyter, Cassandra, Pyspark e Docker"
layout: post
date: 2024-11-27
tags: apache cassandra, nosql, docker, pyspark, jupyter, spark
description: como rodar o apache cassandra e jupyter notebooks com spark localmente com Docker, conectando o jupyter no cassandra
published: true
---

<p align="center">
<img alt="jupyter-notebook" src="../../../assets/images/4/jupyter-notebook.png"/>
</p>

Hoje vou mostrar como configurar um projeto para análise de dados com o Cassandra e o
Jupyter Notebook com Pyspark, usando tudo no Docker. Vamos abordar:

- [Configurando o Apache Cassandra com Docker](#1)
- [O que é o Spark?](#2)
- [Configurando o Jupyter com Pyspark no Docker](#3)
- [Iniciando um sessão Spark conectada no Cassandra](#4)

Antes de começar, essa é a estrutura de pastas do projetinho:

```
cassandra-jupyter-spark-python
├── .gitignore
├── Makefile
├── README.md
├── data
│   └── csv-here.md
├── docker-compose.yml
├── jupyter
│   └── Untitled.ipynb
└── pyproject.toml
```

Você pode encontrar um projeto similiar a esse, completo, com análise de dados (porcamente
feita por mim) no [GitHub](https://github.com/nfo94/infraestrutura-cassandra-pd).

## <a name="1"></a>Configurando o Apache Cassandra com Docker

O Cassandra é um **banco de dados NoSQL decentralizado, com escalabilidade elástica,
altamente disponível, tolerante a falhas e orientado a linhas particionadas**; escrevi
um [texto sobre o Cassandra aqui](https://nfo94.github.io/2024/10/27/o-que-e-o-apache-cassandra-e-quando-usar.html)
e você pode verificar a [documentação do Cassandra aqui](https://cassandra.apache.org/doc/latest/).

Para subir o Cassandra com Docker podemos fazer o seguinte:

```yml
services:
  cassandra1:
    image: cassandra:4.0.14
    container_name: cassandra1
    networks:
      - cassandra_network
    environment:
      - CASSANDRA_CLUSTER_NAME=cassandra_cluster
      - CASSANDRA_SEEDS=cassandra1
      - CASSANDRA_DC=datacenter1
      - CASSANDRA_RACK=rack1
      - CASSANDRA_ENDPOINT_SNITCH=GossipingPropertyFileSnitch
    volumes:
      - ./data:/data
    ports:
      - "9042:9042"
```

No código acima estamos criando um node do Cassandra 4.0.14 (versão escolhida para evitar
problemas de compatibilidade nesse projeto) numa rede Docker chamada `cassandra_network`.
Explicando as variáveis de ambiente no `environment`:

- `CASSANDRA_CLUSTER_NAME`: define o nome do cluster onde um ou mais nodes do Cassandra
  irão rodar. Cluster se refere a mais de uma máquina, geralmente para se referir a um
  sistema distribuído. No “docker-compose” do projeto chamamos de “cassandra_cluster”;
- `CASSANDRA_SEEDS`: indica os serviços (ou IPs) usados pelo gossip do Cassandra, para
  adicionar novos nodes ao cluster. O gossip é um protocolo utilizado para manter
  informações sobre o status dos nodes no cluster, para dar suporte a descentralização e
  tolerância a particionamento;
- `CASSANDRA_DC`: define o data center do node, que é um agrupamento lógico de racks. Não
  é necessário para rodar essa infraestrutura localmente em um notebook, como é o caso
  atual, mas seria utilizado caso estivéssemos utilizando o Cassandra com o uso adequado
  a que se propõe;
- `CASSANDRA_RACK`: define o rack do node, que é um agrupamento lógico de nodes.
  Novamente, não é necessário para a situação atual, mas seria utilizado em um caso real;
- `CASSANDRA_ENDPOINT_SNITCH`: define a implementação de snitch que esse node irá utilizar.
  O snitch provê informações sobre a topologia da rede onde o Cassandra está para
  redirecionar requisições de forma mais eficiente. Nesse caso aqui configuramos como
  “GossipingPropertyFileSnitch”, geralmente usado em produção, para considerar as
  informações de rack e datacenter.

O volume `./data:/data` serve para que você coloque, por exemplo, arquivos `.csv` parte
da sua análise de dados. No meu caso, [criei um projeto para a pós-graduação](https://github.com/nfo94/infraestrutura-cassandra-pd),
onde usei o comando `COPY` para repassar os dados tratados do `.csv` para o Cassandra.

Agora, criando um segundo node Cassandra:

```yml
cassandra2:
  image: cassandra:4.0.14
  container_name: cassandra2
  networks:
    - cassandra_network
  environment:
    - CASSANDRA_CLUSTER_NAME=cassandra_cluster
    - CASSANDRA_SEEDS=cassandra1
    - CASSANDRA_DC=datacenter1
    - CASSANDRA_RACK=rack1
    - CASSANDRA_ENDPOINT_SNITCH=GossipingPropertyFileSnitch
  volumes:
    - ./data:/data
  ports:
    - "9043:9042"
  depends_on:
    - cassandra1
```

Você pode usar somente um node, já que está rodando em seu computador, mas vale lembrar
que um node só perde o propósito de uso do Cassandra. Esse banco é feito para ter diversos
nodes no cluster.

Veja que esse segundo node aponta os seeds para o outro node. Isso é para que o `cassandra2`
se conecte com o `cassandra1`, para descrobrir a topologia do cluster e se unir a esse
cluster.

## <a name="2"></a>O que é o Spark?

O [Apache Spark](https://spark.apache.org/) é uma engine para engenharia e análise de
dados, ciência de dados e machine learning em larga escala, onde é possível rodar para
máquinas com um único nó ou cluster. Podemos usar Python, SQL, Scala, Java e R para
interagir com o Spark, e aqui nesse projeto usaremos Python, com Pyspark.

Usos do Spark, segundo sua documentação:

- Batch/streaming: unifica o processamento dos dados em lotes e streaming em tempo real,
  usando Python, SQL, Scala, Java ou R
- Análise de SQL: executa consultas ANSI SQL rápidas e distribuídas para painéis e
  relatórios ad-hoc
- Ciência de dados em escala: executa Análise Exploratória de Dados (EDA) em dados em
  escala de petabytes sem precisar recorrer à redução de amostragem
- Aprendizado de máquina: treine algoritmos de aprendizado de máquina em um laptop e use
  o mesmo código para escalar para clusters tolerantes a falhas de milhares de máquinas.

## <a name="3"></a>Configurando o Jupyter com Pyspark no Docker

O Jupyter é uma tecnologia para computação interativa onde usamos uma linguagem de
programação para, geralmente, fazer análises de dados e experimentos do tipo. Para rodá-lo
no Docker:

```yml
  # Docker Hub Image. Uses spark-3.5.0-bin-hadoop3
  jupyter_pyspark:
    image: jupyter/pyspark-notebook:latest
    container_name: juptyer_pyspark
    environment:
      JUPYTER_ENABLE_LAB: "yes"
    ports:
      - "8888:8888"
    networks:
      - cassandra_network
    volumes:
      - ./jupyter:/home/jovyan
    depends_on:
      - cassandra1

networks:
  cassandra_network:
    driver: bridge
```

Essa imagem do Jupyter já vem com o Spark, mas vale apontar que aqui estamos usando a
imagem do [Docker Hub](https://hub.docker.com/r/jupyter/pyspark-notebook/), que não
recebe atualizações e está parada no Spark 3.5.0 e Hadoop 3, e não da [Quay](https://quay.io/repository/jupyter/pyspark-notebook),
que recebe atualizações. Tive muitos problemas tentando manter a compatibilidade entre
as ferramentas, e a imagem do Docker Hub funcionou para o propósito.

O volume, assim como nos cassandras, é para poder salvar os notebooks no host. O
container está na mesma rede que os outros e o `JUPYTER_ENABLE_LAB` é para que a gente
não precise fazer login ou usar token, bastará abrir o browser no `http://localhost:8888`:

<p align="center">
<img alt="jupyter" src="../../../assets/images/4/jupyter.png"/>
</p>

## <a name="4"></a>Iniciando uma sessão Spark conectada no Cassandra

Com o `docker-compose.yml` finalizado, podemos rodar os containers:

```bash
docker compose up --build
```

No `http://localhost:8888`, abrindo um novo arquivo no Jupyter, vamos importar uma sessão
Spark da lib `pyspark`:

```ipynb
from pyspark.sql import SparkSession
```

E configurar a sessão:

```ipynb
spark = SparkSession.builder.appName("test").config("spark.jars.packages", "com.datastax.spark:spark-cassandra-connector_2.12:3.5.0").config("spark.cassandra.connection.host", "cassandra1").config("spark.cassandra.connection.port", "9042").getOrCreate()
```

Muita atenção aqui nessa parte. O código acima está:

- Iniciando uma sessão Spark
- Criando um novo "app" (o projeto em si) chamado "test"
- Configurando o Spark com um pacote jar, utilizando um conector de Spark para Cassandra
- Conectando com o serviço Cassandra chamado `cassandra1`
- Configurando a porta da conexão para o 9042, que é onde está nosso container
- Criando a conexão

Note que estamos usando um conector específico:

```txt
spark-cassandra-connector_2.12:3.5.0
```

Podemos olhar essas versões [no site do Maven](https://mvnrepository.com/artifact/com.datastax.spark/spark-cassandra-connector),
e aqui nesse ponto foi onde mais topei com erros, especialmente de compatibilidade.

Se tudo der certo no comando anterior, depois rode esse comando para ver a sessão
ativa:

```ipynb
spark.active()
```

Você verá algo assim:

<p align="center">
<img alt="jupyter-pyspark" src="../../../assets/images/4/jupyter-pyspark.png"/>
</p>

---

Fontes:

[Imagem Docker do Cassandra](https://hub.docker.com/_/cassandra/)

[Image Docker do Jupyter com Spark](https://hub.docker.com/r/jupyter/pyspark-notebook/)

[Apache Spark](https://spark.apache.org/)
