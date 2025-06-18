---
title: "SOLID: single responsibility em Go e Python"
layout: post
date: 2025-06-23
tags: solid, single responsibility principle, python, go
description: explicando o que é o single responsibility principle do SOLID, com exemplos em Python
e em Go
published: true
---

Esse é o primeiro artigo de uma série sobre SOLID explicado com exemplos nas linguagens de
programação Python e Go, da forma mais simples posssível, e iremos utilizar dois livros como base:

- [Hands-On Dependency Injection in Go](https://www.amazon.com.br/Hands-Dependency-Injection-Corey-Scott/dp/1789132762)
- [Clean Code in Python](https://www.amazon.com.br/Clean-Code-Python-maintainable-efficient/dp/1800560214)

Aqui iremos falar sobre o **single responsibility principle**, ou **princípio de responsabilidade
única** em português. Perguntas para guiar nosso estudo:

- [O que é e para que serve o single responsibility principle?](#1)
- [Como aplicar o single responsibility principle em Go?](#2)
- [Como aplicar o single responsibility principle em Python?](#3)

## <a name="1"></a>O que é e para que serve o single responsibility principle?

De todos os princípios SOLID esse é provavelmente o mais simples de entender. A ideia do single
responsibility principle é **reduzir complexidade decompondo o código em partes menores, mais fáceis
de entender, constatando que um componente de software, uma classe, por exemplo, deve ter apenas uma
responsabilidade**. Esse princípio está relacionado ao **separation of concerns**, onde temos a ideia
de manter a coesão do código definindo bem quais são as "preocupações", ou responsabilidades, que
aquele componente possui.

<div align="center">
<img alt="solid" src="../../../assets/images/8/solid.png"/>
</div>

---

Fontes:

SCOTT, Corey. [Hands-On Dependency Injection in Go](https://www.amazon.com.br/Hands-Dependency-Injection-Corey-Scott/dp/1789132762).
Packt, 2018.

ANAYA, Mariano. [Clean Code in Python](https://www.amazon.com.br/Clean-Code-Python-maintainable-efficient/dp/1800560214).
Packt, 2021.
