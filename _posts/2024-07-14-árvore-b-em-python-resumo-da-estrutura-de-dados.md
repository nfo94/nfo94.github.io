---
title: "Árvore B (b tree) em Python: resumo da estrutura de dados"
layout: post
date: 2024-07-14
tags: python b-tree árvore-b estruturas-de-dados
description: resumo sobre estruturas de dados árvore b, com exemplos na linguagem Python
published: true
---

Este artigo aborda de forma prática a estrutura de dados árvore b (b tree) com exemplos na
linguagem Python. Precisei estudar a estrutura para entender melhor o livro [Database Internals do
Alex Petrov](https://www.amazon.com.br/Database-Internals-Distributed-Systems-English-ebook/dp/B07XW76VHZ/ref=tmm_kin_swatch_0?_encoding=UTF8&dib_tag=se&dib=eyJ2IjoiMSJ9.iKPDI1kEgUXPUM7fVhNzHBRKU60ECB7yYS_arvp13YVO7ljfQVz92YOwtP4pFGLjpJbscRV8cFm0yoGYmEOCIqnEt6CmalQy_ypBtFNZU5Dw6X1pyFV3_tzMiUcu6GTcVa81G6l3oVjBDyS2dq1ET5cx_UClN74eYlBGSjZ03pVUOddEraJ6htVhRUF-rAXCjs7EKvV991dWxAZLXzFteq5XGh-SBwTy6esZHD0wB6IrjeF53KXPafpSiDHes3esUf8HEbOewYZGdK2p8NS0LcoSSdUgvGNBTuUONERr9hg.5nzu0Qf-Mlcxt9QL0h8nwEGoJsb83kY0Fk3EmYcvtko&qid=1721200648&sr=8-1#customerReviews). Abordarei aqui:

- [O que é uma árvore em computação?](#1)
- [O que é uma árvore b (b tree)?](#2)
- [Onde a estrutura de dados árvore b é utilizada na prática?](#3)
- [Operações na árvore b](#4)

## <a name="1"></a>O que é uma árvore em computação?

Uma árvore em ciência da computação é uma estrutura de dados não linear onde os dados estão
dispostos de forma hierárquica como, por exemplo, numa árvore genealógica:

<p align="center">
<img src="../../../assets/images/abeprdedd/exemplo-de-árvore.svg" alt="exemplo de árvore" />
<figcaption>Exemplo de árvore do Wikipedia. <a href="https://donate.wikimedia.org/w/index.php?title=Special:LandingPage&country=BR&uselang=pt-br&wmf_campaign=C11_Waystogive&wmf_medium=Waystogive&wmf_source=Waystogive">Doe para o Wikipedia</a>
</figcaption>
</p>

Uma árvore tem nodos/nós (nodes) e todo nó tem um pai, exceto a raíz (root). Cada nó pode ter
filhos ou não. Podemos ter nós internos, ou seja, que têm filhos, ou nós externos (leaf), que não
tem filhos.

<p align="center">
<img src="../../../assets/images/abeprdedd/componentes-de-árvores.svg" alt="exemplo de árvore" />
<figcaption>Componentes de uma árvore do livro da Loiane Groner. <a href="https://www.amazon.com.br/Estruturas-Dados-Algoritmos-Com-Javascript/dp/8575226932">Compre o livro aqui</a>
</figcaption>
</p>

## <a name="2"></a>O que é uma árvore b (b tree)?

O computador pode ler um bloco de informação do disco rígido conhecido como _página_ (page) e
guardar essa informação na memória, e assim podemos, por exemplo, começar a ver um "pedaço" de um
vídeo que ainda está sendo baixado. Ou se você for uma pessoa mais velha, pode lembrar de quando
era necessário usar mais de um CD para instalar um jogo no PC, o famoso "Insira o disco 2 de 3".

A estrutura árvore b foi projetada por Rudolf Bayer e Edward levando em conta o custo de recuperação
de novos blocos de dados. É um tipo de árvore auto-balanceada que mantém dados ordenados e permite
pesquisa, acesso sequencial, inserção e exclusão em O(log n) (complexidade logaritmica):

<p align="center">
<img src="../../../assets/images/abeprdedd/complexidade-computacional.svg" alt="complexidade computacional" />
<figcaption>Complexidade computacional. <a href="https://donate.wikimedia.org/w/index.php?title=Special:LandingPage&country=BR&uselang=pt-br&wmf_campaign=C11_Waystogive&wmf_medium=Waystogive&wmf_source=Waystogive">Doe para o Wikipedia</a> 
</figcaption>
</p>

## <a name="3"></a>Onde a estrutura de dados árvore b é utilizada?

## <a name="4"></a>Operações na árvore b

---

Fontes:

KUBICA, Jeremy. [Data Structures the Fun Way: An Amusing Adventure with Coffee-Filled Examples](https://www.amazon.com.br/Data-Structures-Fun-Way-Coffee-Filled-ebook/dp/B09WJYH4KL/ref=sr_1_1?__mk_pt_BR=%C3%85M%C3%85%C5%BD%C3%95%C3%91&crid=3JQIDS5K8P3GG&dib=eyJ2IjoiMSJ9.QIWC9DMjgGndOeVZ8TueG1NaaAaeBHi_f43t0_vGziQTLLJp3WHQWyju-elvYw3qqODWLKP7E64irB6Wvb8SHA8MGwS5V4BEUtJWmlhHwpblKy9uwPzhqmu9ILCM7gfevK9lNZWoeVaehXegpqdOEcXogE8KIuElJ5QAbharQmF8TGBYIo4tDxEBbkwXeH0TlCLsw5oQ0N49pF1aIvOKiNxDmIKvukMwhcil63cYEq1eB0TrX67VKrf7HH2rNJDX8NL1YBS3nxHP7smOM8edhYArDxGP1FWUrVXXzKFDMaQ.BETUaToSJ1iIHnBo9FftF4rJ29wpoaa74M_Yv6cZdcA&dib_tag=se&keywords=data+structures+the+fun+way&qid=1721200845&sprefix=data+structures+the+fun+way%2Caps%2C148&sr=8-1).
No Starch Press, 2022.

GRONER, Loiane. [Estruturas de Dados e Algoritmos com JavaScript: Escreva um Código JavaScript
Complexo e Eficaz Usando a Mais Recente ECMAScript](https://www.amazon.com.br/Estruturas-Dados-Algoritmos-Com-Javascript/dp/8575226932). São Paulo: Novatec, 2019.

[Árvore B por Wikipedia](https://pt.wikipedia.org/wiki/%C3%81rvore_B)

[Introduction of B-Tree por Geeks for Geeks](https://www.geeksforgeeks.org/introduction-of-b-tree-2/)

[Árvore por Wikipedia](<https://pt.wikipedia.org/wiki/%C3%81rvore_(estrutura_de_dados)>)
