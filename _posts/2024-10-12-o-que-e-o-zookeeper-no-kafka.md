---
title: "O que é o Zookeeper no Kafka?"
layout: post
date: 2024-10-12
tags: zookeeper kafka
description: resumo sobre zookeeper no apache kafka, sobre algoritmos de 
consenso e sistemas distribuídos
published: false
---

Quando comecei a estudar o [Apache Kafka](https://kafka.apache.org/) me lembro
de ter decorado que "o Kafka precisa do Zookeeper para funcionar", mas eu não
entendia a fundo o porquê. Agora, depois de algum tempo lidando com o Kafka,
depois de ter lido alguns livros e mais recentemente parado de usar o
[Zookeeper](https://zookeeper.apache.org/) em um projeto no trabalho, consigo
entender um pouco melhor essa tecnologia.

Vamos retomar brevemente alguns conceitos antes de adentrar o Zookeeper, seguindo
esses tópicos:

- [O que é o Apache Kafka?](#1)
- [O que é um sistema distribuído?](#2)
- [Utilidades e problemas em sistemas distribuídos](#3)
- [O que é o Zookeeper?](#4)
- [Como o Zookeeper funciona?](#5)
- [ZAB](#6)

## <a name="1"></a>O que é o Apache Kafka?

Sabemos que o Kafka é um "plataforma de streaming de eventos distribuídos". De
acordo com a [documentação da tecnologia](https://kafka.apache.org/documentation/#intro_nutshell):

> **Kafka é um sistema distribuído que consiste em servidores e clientes que se
> comunicam por meio de um protocolo de rede** TCP de alto desempenho . Ele pode ser
> implantado em hardware bare-metal, máquinas virtuais e contêineres em ambientes
> locais e de nuvem.

Imagem retirada do livro [Kafka The Definitive Guide](https://www.amazon.com.br/Kafka-Definitive-English-Gwen-Shapira-ebook/dp/B09L6KLWDG/ref=sr_1_1?crid=34541GOF2XE8O&dib=eyJ2IjoiMSJ9.II4vBwuTE8Ztv-B4Vev1mD6iGEgc8hRpqJcOgcWcuSK1FN0OIz5syeX4m1MGoEjZBM6mworGxZoN2vdcNs1yL584sAqhvdbrZRmq70DfS35fQG-vcLd7VtnnqX-HCIu7JJPhkkEGOvU-XsS-q0mAKXTfEX_YUIJ62EPLVWcHMUOLhfT6KnOY_P2ufen4VX_RUMnSY2QPRQ1ZEYwmbX30klBD3Om4sUX_tX6HjIUOaZbk7Bmdtw6-SdI9QYksqFqKRsO9oIDWPDufk5jhpFqoI3X9NH3ufUA1df6HpWWL43w.dG9nUCmtdYisl6pkEU4tUekSDEdelhMdX3K6ibkmbnM&dib_tag=se&keywords=kafka+the+definitive+guide&qid=1727991294&sprefix=kafka+the+%2Caps%2C209&sr=8-1):

<p align="center">
<img alt="kafka" src="../../../assets/images/2/kafka.png" alt="kafka" />
</p>

Sobre os servidores:

> O Kafka é executado como um cluster de um ou mais servidores que podem
> abranger vários datacenters ou regiões de nuvem.

Sobre os clientes:

> Eles permitem que você escreva aplicativos distribuídos e microsserviços que
> leem, escrevem e processam fluxos de eventos em paralelo, em escala e de forma
> tolerante a falhas, mesmo no caso de problemas de rede ou falhas de máquina

Em suma, entendemos que o Kafka é um _sistema distribuído_ de clientes e
servidores se comunicando por uma rede de computadores. Mas o que é, de fato, um
sistema distribuído?

## <a name="2"></a>O que é um sistema distribuído?

Segundo Sukumar Ghosh no livro [Distributed Systems](https://www.amazon.com.br/Distributed-Systems-Algorithmic-Approach-Information-ebook/dp/B08LGGWQD6/ref=tmm_kin_swatch_0?_encoding=UTF8&dib_tag=se&dib=eyJ2IjoiMSJ9.3lQpMqrqJTUh2iGJBpi01U5pZHL1HAiE9ROs2XXOX_QpVtmuV5VhTuPceIdx4wMKabcodcbUt_sTEXYCmDqBF8HTSx4e8S1y_LKmLNQ-A6DQr6JR2AYoxPi_T0GOfrLOd6zjSfZ9pqUnxNVwvEO-EnLss0mEbRVZRgrhKymBbzDLIjgCgWQHm8CXfIGZQG_uKoSF0EhwWSPmp1D5pg7AOhvorK0cTXBVPpAUjmqtcDe-RPkzVBENRIlfuBKulp5Xp8c783hSh4E9KYDTO3HJ45LkbStHYwl4b0JpZSrR5wg.Q9uPCm_ot8tV87xs7L9_rXMkLXeUNT25gRFsmhWi5tI&qid=1727992593&sr=8-16), um sistema
distribuído segue alguns critérios:

- **Processos múltiplos**: O sistema consiste em mais de um processo sequencial.
- **Comunicação entre processos**: Os processos se comunicam entre si usando
  mensagens que levam um tempo finito para viajar de um processo para outro.
- **Espaços de endereço disjuntos**: Os processos têm espaços de endereço disjuntos.
- **Objetivo coletivo**: Os processos devem interagir entre si para atingir um objetivo comum.

Podemos pensar então num sistema distribuído como vários computadores trabalhando
em conjunto com um objetivo comum.

## <a name="3"></a>Utilidades e problemas em sistemas distribuídos

Entendemos o que é um sistema distribuído, mas qual a sua utilidade, por que
evoluímos para isso? Segundo Ghosh:

- **Ambiente geograficamente distribuído**: Primeiro, em muitas situações, o próprio
  ambiente de computação é geograficamente distribuído (exemplo: usuários espalhados
  pelo mundo).
- **Acelerar**: Segundo, há a necessidade de acelerar a computação. A velocidade da
  computação em uniprocessadores tradicionais está se aproximando rapidamente do
  limite físico.
- **Compartilhamento de recursos**: Terceiro, há uma necessidade de compartilhamento
  de recursos. Aqui, o termo recurso representa recursos de hardware e software.
- **Tolerância a falhas**: Quarto, uniprocessadores poderosos ou sistemas de
  computação construídos em torno de um único nó central são propensos a um
  colapso completo quando o processador falha.

Um sistema distribuído nos permite escalar horizontalmente, ou seja, adicionar
mais máquina a um sistema e distribuir o trabalho entre elas. Escalar
verticalmente, ou seja, adicionar mais recursos computacionais como adicionar
mais memória RAM ou cores de CPU a uma única máquina, geralmente é mais caro e
continua tendo um único nó de possível falha, o que não é interessante.

Mas essa arquitetura não tem só benefícios. O problema que temos interesse em
discutir aqui é o que irá nos levar ao ponto central desse texto, o Zookeeper:

**Como coordenar o trabalho dessas diferentes máquinas, se cada uma pode ter seus
próprios recursos, dados, contexto, horário (timezone) etc? Como fazê-las entrar
em consenso?**

## <a name="4"></a>O que é o Zookeeper?

O Zookeeper serve justamente para resolver alguns desses problemas de coordenação
de um sistema distribuído. Ao invés de perder tempo escrevendo a lógica de
coordenação dessas máquinas,você pode utilizar o Zookeeper para isso e se
concentrar em escrever lógica de negócio. Foi o que aconteceu no projeto do Kafka.

Segundo Benjamin Reed e Flavio Junqueira no livro [ZooKeeper: Distributed Process Coordination](https://www.amazon.com.br/ZooKeeper-Distributed-Process-Coordination-English-ebook/dp/B00GRCODKS/ref=sr_1_11?__mk_pt_BR=%C3%85M%C3%85%C5%BD%C3%95%C3%91&crid=AO9B67BXTZU6&dib=eyJ2IjoiMSJ9.LDY9vzKRWpcyvZPCR9vkHB8Y021y4hS486pn6sxAadoRySYcH12HftDmUAp4ca5RQCvsNxwhlzpHkEFJJ8x3smFzkJZN85regDC9W-Dx0C3cN8jKHMicPQJrA7gNkVcM8UyNSjyA8YrHgX9i7vZjtIraSNVK0E-im8_ZxhwkDAWEHZ1jUAA5qUXn3iSBIHpZSecROkbNNpAkbUoMNKnsEemOm6My2nSQG_I4e9uWkzu1nVPeS-_nt87l1ld0Y3UBNxakUE0HXpyamS3KtxxcCaHD0-gfJ0-kh-8ygZMwpb8.oIirvM0xo9lssoJ3iE7qa9uFbsWH26NpLm7Yv6OixSg&dib_tag=se&keywords=zookeeper&qid=1727988255&sprefix=zookeeper%2Caps%2C165&sr=8-11):

> O ZooKeeper foi projetado para ser um serviço robusto que permite que
> desenvolvedores de aplicativos se concentrem principalmente na lógica de seus
> aplicativos em vez da coordenação. Ele expõe uma API simples, inspirada na API
> de um sistema de arquivos, que permite que desenvolvedores implementem tarefas
> comuns de coordenação, como eleger um servidor mestre, gerenciar associação de
> grupo e gerenciar metadados.

[O Kafka utiliza o Zookeeper para os seguintes pontos](https://zookeeper.apache.org/doc/current/zookeeperUseCases.html):

- **Eleição do controlador (leader election)**: O controlador é uma das entidades
  de corretagem mais importantes em um ecossistema Kafka e também tem a
  responsabilidade de manter o relacionamento líder-seguidor em todas as partições.
- **Configuração de tópicos**: A configuração referente a todos os tópicos, incluindo a
  lista de tópicos existentes, o número de partições para cada tópico, a
  localização de todas as réplicas, a lista de substituições de configuração para
  todos os tópicos e qual nó é o líder preferido, etc.
- **Listas de controle de acesso**: Listas de controle de acesso ou ACLs para todos os
  tópicos também são mantidas no Zookeeper.
- **A associação (membership) ao cluster**: Zookeeper também mantém uma lista de
  todos os brokers que estão funcionando em um determinado momento e fazem parte do
  cluster.

---

Fontes:

REED, Benjamin; JUNQUEIRA, Flavio. [ZooKeeper: Distributed Process Coordination](https://www.amazon.com.br/ZooKeeper-Distributed-Process-Coordination-English-ebook/dp/B00GRCODKS/ref=sr_1_11?__mk_pt_BR=%C3%85M%C3%85%C5%BD%C3%95%C3%91&crid=AO9B67BXTZU6&dib=eyJ2IjoiMSJ9.LDY9vzKRWpcyvZPCR9vkHB8Y021y4hS486pn6sxAadoRySYcH12HftDmUAp4ca5RQCvsNxwhlzpHkEFJJ8x3smFzkJZN85regDC9W-Dx0C3cN8jKHMicPQJrA7gNkVcM8UyNSjyA8YrHgX9i7vZjtIraSNVK0E-im8_ZxhwkDAWEHZ1jUAA5qUXn3iSBIHpZSecROkbNNpAkbUoMNKnsEemOm6My2nSQG_I4e9uWkzu1nVPeS-_nt87l1ld0Y3UBNxakUE0HXpyamS3KtxxcCaHD0-gfJ0-kh-8ygZMwpb8.oIirvM0xo9lssoJ3iE7qa9uFbsWH26NpLm7Yv6OixSg&dib_tag=se&keywords=zookeeper&qid=1727988255&sprefix=zookeeper%2Caps%2C165&sr=8-11). O'Reilly, 2013.

GHOSH, Sukumar. [Distributed Systems](https://www.amazon.com.br/Distributed-Systems-Algorithmic-Approach-Information-ebook/dp/B08LGGWQD6/ref=tmm_kin_swatch_0?_encoding=UTF8&dib_tag=se&dib=eyJ2IjoiMSJ9.3lQpMqrqJTUh2iGJBpi01U5pZHL1HAiE9ROs2XXOX_QpVtmuV5VhTuPceIdx4wMKabcodcbUt_sTEXYCmDqBF8HTSx4e8S1y_LKmLNQ-A6DQr6JR2AYoxPi_T0GOfrLOd6zjSfZ9pqUnxNVwvEO-EnLss0mEbRVZRgrhKymBbzDLIjgCgWQHm8CXfIGZQG_uKoSF0EhwWSPmp1D5pg7AOhvorK0cTXBVPpAUjmqtcDe-RPkzVBENRIlfuBKulp5Xp8c783hSh4E9KYDTO3HJ45LkbStHYwl4b0JpZSrR5wg.Q9uPCm_ot8tV87xs7L9_rXMkLXeUNT25gRFsmhWi5tI&qid=1727992593&sr=8-16). Chapman and
Hall/CRC, 2014.

[Site do Apache Kafka](https://kafka.apache.org/)

[Site do Zookeeper](https://zookeeper.apache.org/)
