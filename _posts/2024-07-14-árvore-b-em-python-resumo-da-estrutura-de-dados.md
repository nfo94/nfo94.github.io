---
title: "Árvore B (b-tree) em Python: resumo da estrutura de dados"
layout: post
date: 2024-07-14
tags: python b-tree árvore-b estruturas-de-dados
description: resumo sobre estruturas de dados árvore b, com exemplos na linguagem Python
published: true
---

Este artigo aborda de forma prática a estrutura de dados árvore b (b-tree) com exemplos na
linguagem Python. Abordarei aqui:

- [O que é uma árvore em computação?](#1)
- [O que é uma árvore b (b-tree)?](#2)
- [Onde a estrutura de dados árvore b é utilizada na prática?](#3)
- [Operações na árvore b](#4)

## <a name="1"></a>O que é uma árvore em computação?

Uma árvore em ciência da computação é \*\*uma estrutura de dados não linear onde os dados estão
dispostos de forma hierárquica\*\* como, por exemplo, numa árvore genealógica:

<p align="center">
<img src="../../../assets/images/abeprdedd/exemplo-de-arvore.png" alt="exemplo de árvore" />
<figcaption>Exemplo de árvore do Wikipedia. <a href="https://donate.wikimedia.org/w/index.php?title=Special:LandingPage&country=BR&uselang=pt-br&wmf_campaign=C11_Waystogive&wmf_medium=Waystogive&wmf_source=Waystogive">Doe para o Wikipedia</a>
</figcaption>
</p>

Uma árvore tem **nodos/nós (nodes)** e todo nó tem um pai, exceto a **raíz (root)**. Cada nó pode
ter filhos ou não. Podemos ter **nós internos**, ou seja, que têm filhos, ou **nós externos (leaf)**, que não tem filhos. Chamamos de **subárvore** um nó e seus ancestrais.

<p align="center">
<img src="../../../assets/images/abeprdedd/componentes-de-árvores.png" alt="componentes de uma árvore" />
<figcaption>Componentes de uma árvore do livro da Loiane Groner.</figcaption>
</p>

## <a name="2"></a>O que é uma árvore b (b-tree)?

O computador pode ler um bloco de informação do disco rígido conhecido como _página_ (page) e
guardar essa informação na memória, e assim podemos, por exemplo, começar a ver um "pedaço" de um
vídeo que ainda está sendo baixado. Ou se você for uma pessoa mais velha, pode lembrar de quando
era necessário usar mais de um CD para instalar um jogo no PC, o famoso "Insira o disco 2 de 3".

A estrutura árvore b foi projetada por Rudolf Bayer e Edward levando em conta o custo de recuperação
de novos blocos de dados. É um tipo de **árvore auto-balanceada** que mantém dados ordenados e
permite busca, inserção e exclusão em O(log n) (complexidade logaritmica):

<p align="center">
<img src="../../../assets/images/abeprdedd/complexidade-computacional.svg" alt="complexidade computacional" />
<figcaption>Complexidade computacional. <a href="https://donate.wikimedia.org/w/index.php?title=Special:LandingPage&country=BR&uselang=pt-br&wmf_campaign=C11_Waystogive&wmf_medium=Waystogive&wmf_source=Waystogive">Doe para o Wikipedia</a> 
</figcaption>
</p>

Dizer que uma árvore é auto-balanceada significa que ela reforça cada nó a ter um mínimo número de
chaves, e que ela evita que um nó tenha muito mais níveis do que o resto. Abaixo um exemplo de
árvore não balanceada:

<p align="center">
<img src="../../../assets/images/abeprdedd/árvore-não-balanceada.png" alt="árvore não balanceada" />
<figcaption>Exemplo de árvore não balanceada do livro da Loiane Groner.</figcaption>
</p>

Uma b-tree permite aos nós internos terem mais de uma chave e mais de uma branch (ramo). Ou seja, a b-tree nos permite guardar vários pedaços de dados em um único nó. Exemplo de b-tree:

<p align="center">
<img src="../../../assets/images/abeprdedd/exemplo-de-b-tree.png" alt="exemplo de b-tree" />
<figcaption>Exemplo de árvore b do livro do Jeremy Kubica.</figcaption>
</p>

Definimos o tamanho do nó da árvore B com um parâmetro de tamanho _k_, e todos os nós guardam entre
k e 2k de chaves ordenadas, enquanto a raíz pode guardar entre 0 e 2k.

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
