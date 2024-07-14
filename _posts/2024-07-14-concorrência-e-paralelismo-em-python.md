---
title: "Concorrência e paralelismo em Python"
layout: post
date: 2024-07-14
tags: python asyncio threading multiprocessing concorrência paralelismo
published: false
---
O objetivo deste texto é dar um resumo direto ao ponto dos **conceitos básicos necessários para entender sobre concorrência a paralelismo na linguagem Python**. Recomendo ter uma base mínima sobre o assunto ou aliar esse texto com estudo em outras fontes. Todas as referências estão ao final do texto. 

Abordarei os seguintes tópicos:

- [O que é um processo?](#1)
- [O que são threads?](#2)
- [O que significa I/O bound e CPU bound?](#3)
- [O que é o GIL do Python?](#4)
- [O que é concorrência?](#5)
- [O que é paralelismo?](#6)
- [A biblioteca asyncio](#7)
- [A biblioteca threading](#8)
- [A biblioteca multiprocessing](#9)

## <a name="1"></a>O que é um processo?
Em computação um **processo é uma instância de uma aplicação rodando**. Se você abrir uma aplicação no seu computador, como o navegador, essa aplicação vai estar associada a algum processo. Um processo é composto por:

- Contexto de hardware: armazena conteúdo de registradores gerais e específicos da CPU
- Contexto de software: especifica os recursos que podem ser alocados pelo processo
- Espaço de endereçamento: especifica a área da memória que o processo pertence

A imagem a seguir foi retirada do [livro do Francis Machado e do Luis Maia](https://www.amazon.com.br/Arquitetura-Sistemas-Operacionais-Incluindo-Exerc%C3%ADcios/dp/8521622104/ref=sr_1_1?__mk_pt_BR=%C3%85M%C3%85%C5%BD%C3%95%C3%91&crid=150WW8OAI7BK3&dib=eyJ2IjoiMSJ9.A1ZhX8ImePrgue4fqDmOFhTfVbkIf5kIlU2jq5kd4laG4KvRRBXQekMR1rhx34OdkcpofR8kV8Ln0SjtzbN9on9rfe1wq8VNaqPBEYyuFuE.byPfWCKB9260AyrDAXjLab022xEJbcexS5jc_qZgex0&dib_tag=se&keywords=Arquitetura+de+Sistemas+Operacionais%3A+Incluindo+Exerc%C3%ADcios+com+o+Simulador+SOSIM+e+Quest%C3%B5es+do+ENADE&qid=1720896386&sprefix=arquitetura+de+sistemas+operacionais+incluindo+exerc%C3%ADcios+com+o+simulador+sosim+e+quest%C3%B5es+do+enade%2Caps%2C137&sr=8-1):

![anatomia de um processo](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/y2082mv737qlohp0fxf8.png)

Essas informações são necessárias para a execução de um programa.

## <a name="2"></a>O que são threads?
Uma **thread é uma sub-rotina de um programa, sendo a menor unidade de execução que um sistema operacional gerencia e componente de um processo**.

As várias threads de um processo hipotético podem ser executadas concorrentemente (que entenderemos em breve), compartilhando recursos como memória. Diferentes processos não compartilham esses recursos.

A imagem abaixo foi retirada do [Wikipedia](https://en.wikipedia.org/wiki/File:Concepts-_Program_vs._Process_vs._Thread.jpg#filelinks):

![programa, processo e thread](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/q5yavkfyetesdrijxfl8.jpg)

Interpretando a imagem acima, podemos extrair que um programa fica salvo em disco (memória secundária, não-volátil) e inclui várias instruções, podendo ser instanciado (iniciado) em um ou mais processos, e esses por sua vez podem ter várias threads associadas.

## <a name="3"></a>O que significa I/O bound e CPU bound?
Essas duas expressões aparecem bastante na discussão sobre concorrência e podem aparecer em português com E/S (entrada/saída) e UCP (unidade central de processamento). 

Quando falamos sobre I/O bound e CPU bound estamos falando dos fatores limitantes que previnem uma operação de rodar mais rápido em nosso computador, e podemos encontrar esse dois tipos de operações na mesma codebase.

**Uma operação CPU bound faz uso intenso da CPU, e rodará mais rápido se a CPU for mais poderosa**. Ou seja, se formos de 2GHz para 4GHz de velocidade de clock essa operação provavelmente rodará mais rápido. Estamos falando aqui de operações que realizam muitas computações, cálculos; a exemplo, como calcular Pi.

**Uma operação I/O bound depende da velocidade da rede e velocidade dos dispositivos de entrada e saída**. Fazer um request a um servidor web ou ler um arquivo do disco são operações I/O bound.

Ambos os tipos de operações podem se beneficiar do uso de concorrência.

## <a name="4"></a>O que é o GIL do Python?
GIL significa **global interpreter lock (bloqueio do interpretador global), cujo objetivo é prevenir um processo Python de executar mais de um bytecode de instrução Python ao mesmo tempo**. Para rodar uma thread é necessário "adiquirir" o GIL e enquanto uma thread detém o GIL outra thread não pode adiquiri-lo ao mesmo tempo. Isso não significa que não podemos ter mais de uma thread nesse contexto. 

Aqui estamos considerando a implementação de referência do Python. **O CPython é a implementação padrão do Python**, usada como referência de como a linguagem se comporta. Existem outras implementações, como Jython ou IronPython. O GIL está presente no CPython e só recentemente tivemos uma [PEP (Python Enhancement Proposal - proposta de melhoria do Python) propondo tornar o GIL opcional](https://peps.python.org/pep-0703/).

A ideia do GIL é prevenir _race conditions_, que podem surgir quando mais de uma thread precisa referenciar um objeto Python ao mesmo tempo. Se mais de uma thread modificar uma variável compartilhada essa variável pode ficar em um estado inesperado. Imagem retirada do [livro do Matthew Fowler](https://www.amazon.com.br/Python-Concurrency-Asyncio-Matthew-Fowler/dp/1617298662/ref=sr_1_3?__mk_pt_BR=%C3%85M%C3%85%C5%BD%C3%95%C3%91&crid=FRBELDMJHT23&dib=eyJ2IjoiMSJ9.5wyYTHKbZ2idvH8GDqdbsq8qRPv7t5SNNgCQixjEVop7TeR9YRqA66AL9DV1wY3BDFdBJN0pwlU42loLLQGPfFRIKTBDWUW3NzW89oL-TWOnyuyCSLBpYg32aUEyvo5Et8n9sA-Feyh4aMTTeEGydk8r9QKSR-i9FHsBOteOdSn9pQuhlgSHG2YU0jZ4FiaBOXUznz3Ka7XEtQc_ctlNnBN0sDGXPuLqYVgpyhEEYAGP6aTFzdY-SsLaB3duqYq9r15Q6Ux3Zat5I4eqg68T6Gf_jVopvKUv8QJ_je91pOA.eYk31md2uMKA_h8es6XGgFyU1luNGSDMeCdmmaPw0Yc&dib_tag=se&keywords=asyncio&qid=1720900096&sprefix=asyncio%2Caps%2C170&sr=8-3&ufe=app_do%3Aamzn1.fos.a492fd4a-f54d-4e8d-8c31-35e0a04ce61e):

![race condition entre threads](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/t617aj0gqeyybf16k5x1.png)

Na imagem acima duas threads estão tentando incrementar uma reference count simultaneamente, e aí ao invés da contagem dar 2, já que as duas estão incrementando 1, o resultado final dá 1 (cada thread é uma coluna).

## <a name="5"></a>O que é concorrência?
Concorrência em computação acontece quando **lida-se com mais de uma task, sem necessariamente estar lidando com elas exatamente ao mesmo tempo**. Uma frase conhecida do [Rob Pyke](https://en.wikipedia.org/wiki/Rob_Pike) sobre o assunto:

> Concorrência significa **lidar** com muitas coisas ao mesmo tempo. Paralelismo é **fazer** muitas coisas ao mesmo tempo.

Quando falamos de concorrência es

![diagrama de concorrência em Python](./concorrencia-e-paralelismo-em-python/'diagrama de concorrencia.png')



## <a name="6"></a>O que é paralelismo?


## <a name="7"></a>A biblioteca asyncio

## <a name="8"></a>A biblioteca threading

## <a name="9"></a>A biblioteca multiprocessing
 

---
_Fontes_:

FOWLER, Matthew. [Python Concurrency with asyncio](https://www.amazon.com.br/Python-Concurrency-asyncio-English-Matthew-ebook/dp/B09S4NBW2X/ref=tmm_kin_swatch_0?_encoding=UTF8&dib_tag=se&dib=eyJ2IjoiMSJ9.hrCR3O_nnpP3z502Q_U-90OBMrmIMAXl3zIBDIRAa6ZtVFLXDmHRGneAJVIt0nU80CejmcvLhZvK60Jk1LpM3sO1Mqe9MtF1AXr4H3gRLKprHITsENvjoIvmTmfRkV0hF7peJqUAB8EJUejNW-0jVMq4kuzVS_6ku0Q-0Ge1M1V1O147m3K1c1gU8BQwioqpdimWwJBO7TUvxtDEIRjC9ASkmKNr46PqT5JL2jpcK-jbEw-_nYSxPk0lHmW_XBMngMORwj2znV96dfoUXACcfQJ04lRRbHDJmYhkyZNaN4k.IURAGkadqEEUNyjwE5NoLWNseUJm58Vopo-2CV2n5U4&qid=1720896914&sr=8-1). Manning Publications, 2022.
MACHADO, Francis Berenger; MAIA, Luiz Paulo. [Arquitetura de Sistemas Operacionais: Incluindo Exercícios com o Simulador SOSIM e Questões do ENADE](https://www.amazon.com.br/Arquitetura-Sistemas-Operacionais-Incluindo-Exerc%C3%ADcios/dp/8521622104/ref=sr_1_1?__mk_pt_BR=%C3%85M%C3%85%C5%BD%C3%95%C3%91&crid=150WW8OAI7BK3&dib=eyJ2IjoiMSJ9.A1ZhX8ImePrgue4fqDmOFhTfVbkIf5kIlU2jq5kd4laG4KvRRBXQekMR1rhx34OdkcpofR8kV8Ln0SjtzbN9on9rfe1wq8VNaqPBEYyuFuE.byPfWCKB9260AyrDAXjLab022xEJbcexS5jc_qZgex0&dib_tag=se&keywords=Arquitetura+de+Sistemas+Operacionais%3A+Incluindo+Exerc%C3%ADcios+com+o+Simulador+SOSIM+e+Quest%C3%B5es+do+ENADE&qid=1720896386&sprefix=arquitetura+de+sistemas+operacionais+incluindo+exerc%C3%ADcios+com+o+simulador+sosim+e+quest%C3%B5es+do+enade%2Caps%2C137&sr=8-1). Rio de Janeiro: LTC, 2013.
[Thread (computing) by Wikipedia](https://en.wikipedia.org/wiki/Thread_(computing))
