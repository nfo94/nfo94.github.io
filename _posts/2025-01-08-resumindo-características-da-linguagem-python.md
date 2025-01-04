---
title: "Resumindo características da linguagem Python"
layout: post
date: 2025-01-08
tags: python
description: resumindo características de python
published: true
---

<figure align="center">
  <img src="../../../assets/images/6/snake-5483729_640.webp" alt="Imagem de cobra 
  de willypp no Pixabay">
  <figcaption><i>Imagem de <a href="https://pixabay.com/pt/users/willypp-15092123/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=5483729">willypp</a> por <a href="https://pixabay.com/pt//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=5483729">Pixabay</a></i></figcaption>
</figure>

Python é uma linguagem de **propósito geral, multiparadigma, de alto nível, de tipagem
dinâmica, fortemente tipada, que roda em diferentes plataformas e é interpretada (com um
passo de compilação)**. Vamos destrinchar esses conceitos utilizando principalmente o
livro "Python in a Nutshell", documentações da linguagem e as aulas incríveis do [Eduardo
Mendes](https://www.youtube.com/@Dunossauro).

- [Propósito geral e alto nível](#1)
- [Tipagem dinâmica e forte](#2)
- [Linguagem intepretada com um passo de compilação](#3)
- [Roda em diferentes plataformas](#4)

## <a name="1"></a>Propósito geral e alto nível

Consideramos Python como de **propósito geral** pois suas características são úteis
para diversas áreas de software. Iremos encontrar Python em REST APIs, scripts, machine
learning, análises estatísticas, web scraping, etc. Python já era bastante utilizada
na área de inteligência artificial e, com o _boom_ dos LLMs, me parece que muitas áreas
tornamram a linguagem como o padrão de uso. Se procurarmos por vagas de machine learning
engineer, data engineer e por aí vai é muito comum encontrar o Python como requisito.

A consideramos como de **alto nível** por possuir um **alto nível de abstração**, se
afastando bastante da linguagem de máquina que um computador de fato entende. Além disso,
a linguagem é bem "parecida" com uma leitura em inglês, o que é muito atrativo. Outros
exemplos de linguagens de alto nível são Java, JavaScript, entre outras.

## <a name="2"></a>Tipagem dinâmica e forte

Acredito que esse tópico gere uma certa dúvida. Eu particularmente precisei revisitar
esses conceitos diversas vezes até entender. O Python tem **tipagem dinâmica**, ou seja,
não te obriga a declarar o tipo de uma variável, você pode declarar um `a = "texto"` e o
interpretador, por debaixo dos panos, vai inferir que a variável é do tipo `string`. Ao
mesmo tempo, é **fortemente tipada**, pois o interpretador mantém controle dos tipos de
variáveis que estão sendo declaradas no programa, coisa que não acontece numa linguagem
fracamente tipada como o JavaScript, por exemplo. No JavaScript você pode fazer `'x' + 3`
resultar em `'x3'` e não dará erro; se você tentar fazer isso em Python verá:

```bash
TypeError: can only concatenate str (not "int") to str
```

Sobre ser dinamicamente tipada e fortemente tipada ao mesmo tempo, vejamos esse excerto
do [Wiki do Python](https://wiki.python.org/moin/Why%20is%20Python%20a%20dynamic%20language%20and%20also%20a%20strongly%20typed%20language):

> Python é fortemente tipado, pois o interpretador mantém o controle de todos os tipos
> de variáveis. Ele também é muito dinâmico, pois raramente usa o que sabe para limitar o
> uso de variáveis. Em Python, é responsabilidade do programa usar funções internas como
> isinstance() e issubclass() para testar tipos de variáveis ​​e uso correto. Python tenta
> ficar fora do seu caminho enquanto fornece tudo o que você precisa para implementar a
> verificação de tipo forte.

Falando em _"Python tenta ficar fora do seu caminho enquanto fornece tudo o que você
precisa para implementar a verificação de tipo forte"_, pegamos o gancho para falar de
"type hints" ou "**type annotations**". _Type annotations_ são uma feature mais recente
([PEP 484 de de 2014](https://peps.python.org/pep-0484/)) do Python que nos permite
"anotar" o tipo de dado que estamos lidando no código. Do livro "Python in a Nutshell":

> Anotar seu código Python com informações de tipo é uma etapa opcional que pode ser
> muito útil durante o desenvolvimento e a manutenção de um projeto grande ou de uma
> biblioteca. Verificadores de tipo estático e ferramentas lint ajudam a identificar e
> localizar incompatibilidades de tipo de dados em argumentos de função e valores de retorno.
> IDEs podem usar essas anotações de tipo (também chamadas de dicas de tipo) para melhorar
> o preenchimento automático e fornecer documentação pop-up. Pacotes e frameworks de
> terceiros podem usar anotações de tipo para personalizar o comportamento do tempo de
> execução ou para gerar automaticamente código com base em anotações de tipo para métodos
> e variáveis.

É daí que temos libs como [`mypy`](https://github.com/python/mypy) e [`Pydantic`](https://github.com/pydantic/pydantic)
para checar e validar tipos.

## <a name="3"></a>Linguagem intepretada com um passo de compilação

O Python é considerado **interpretado** com um passo de **compilação**: uma linguagem
ser interpretada significa que ela não é executada diretamente pela máquina, e sim é
interpretada e executada por outro programa. Aqui temos um _catch_ que confunde bastante,
pois na real o CPython, que é o interpretador referência da linguagem, é um compilador de
bytecode e também interpretador: ele compila o código Python e, com o resultado dessa
compilação (que chamamos de bytecode ou _intermediary language_), roda esse resultado
diretamente em uma máquina virtual. Recomendo fortemente a [aula do Eduardo Mendes](https://www.youtube.com/watch?v=pxfZTAJDipY&t=946s)
sobre isso:

<div align="center">
<img alt="cpython" src="../../../assets/images/6/cpython.webp"/>
</div>

> _Mas qual a diferença entre esse processo e um processo de compilação de um linguagem
> como Java?_

Para ficar mais claro, veja que em uma linguagem 100% compilada a linguagem intermediária
é transformada para linguagem de máquina, para então ser rodada pelo computador de fato.
Para ilustrar um pouco melhor esse processo acompanhe esse slide da [aula do Eduardo Mendes](https://www.youtube.com/watch?v=pxfZTAJDipY&t=789s):

<div align="center">
<img alt="compilação" src="../../../assets/images/6/compilação.webp"/>
</div>

## <a name="4"></a>Roda em diferentes plataformas

Se você viu essa afirmação em algum lugar, perceba que isso é dito pois você pode escrever
e rodar um programa Python em qualquer computador moderno que tenha Python instalado.
"Instalar" significa ter o interpretador Python para rodar a linguagem. Em computadores
com sistema operacional baseado em Unix, como Linux e MacOS, é comum que o interpretador
Python já esteja instalado em alguma versão. No caso de Windows, até onde sei, você
precisa ir lá no site no Python, baixar e instalar o interpretador.

---

Fontes:

[Python in a Nutshell](https://www.amazon.com.br/Python-Nutshell-English-Alex-Martelli-ebook/dp/B0BRYRD295/ref=tmm_kin_swatch_0?_encoding=UTF8&dib_tag=se&dib=eyJ2IjoiMSJ9.1GrWfyxGQAtCbzVANdvWpIhsGUkbRmGr3ML2fa9FpnVxX7qlzYtkK_-90G8PkjJjgguYnjmO95dfRwRYpxq_KjO-ymBjxJUMcAg0yT84C9fRPz6a3nBhAM8lSKKrFvF3hGlrKUcoMemKQysYgZfgry8iAMCF6td3Hxgn2ewAhQvhBTq7b2NgOPNn467iWo-9eJLV3dAY2oXJNBL2Z2V7IeuYejkKm7JPGhoPmIqu3keAuUEQVk088S7ZYYoK5WLR9GjCa2Do0sSEha3oWfbMz-FHIk0a9t-LOEqeVc2r-Tg.w512AtUskmia2OZ6AWaNIM0RwlmCrPBH-66aAFwmhEI&qid=1732842474&sr=8-1)

[Why is Python a dynamic language and also a strongly typed language](https://wiki.python.org/moin/Why%20is%20Python%20a%20dynamic%20language%20and%20also%20a%20strongly%20typed%20language)

[StackOverflow Compiled vs. Interpreted Languages](https://stackoverflow.com/questions/3265357/compiled-vs-interpreted-languages)

[Como o interpretador do Python funciona? | Live de Python #218](https://www.youtube.com/watch?v=pxfZTAJDipY)
