---
title: "SOLID: open/closed principle em Go e Python"
layout: post
date: 2025-08-04
tags: solid, open/closed principle
description: explicando o que é o open/closed principle do SOLID, com exemplos em Python e em Go
published: true
---

Esse é o segundo artigo de uma série sobre SOLID explicado com exemplos nas linguagens de
programação Python e Go, da forma mais simples posssível, utilizando dois livros como base, um do
Corey Scott e outro do Mariano Anaya:

- [Hands-On Dependency Injection in Go](https://www.amazon.com.br/Hands-Dependency-Injection-Corey-Scott/dp/1789132762)
- [Clean Code in Python](https://www.amazon.com.br/Clean-Code-Python-maintainable-efficient/dp/1800560214)

Aqui iremos falar sobre o **open/closed principle**, ou **princípio aberto/fechado** em português.
Perguntas para guiar nosso estudo:

- [O que é e para que serve o open/closed principle?](#1)
- [Como aplicar o open/closed principle em Python?](#2)
- [Como aplicar o open/closed principle em Go?](#3)

## <a name="1"></a>O que é e para que serve o open/closed principle?

Você já deve ter ouvido a frase **"aberto para extensão, fechado para modificação"**, mas talvez não
tenha entendido perfeitamente o que isso quer dizer. Quando estamos escrevendo uma classe, um módulo,
etc, queremos que seja possível extendê-lo porque **requisitos mudam** e ao longo do tempo sempre surgem
novas features, novas funcionalidades e comportamentos para o nosso software. Quando extender um
software é muito difícil <s>ou parece impossível de ser feito</s>, temos um problema. Em outras palavras,
queremos que seja fácil adicionar comportamentos, mas não queremos modificar o que já existe (podendo
quebrar funcionalidades), e é justamente para isso que serve o princípio open/closed.

Corey Scott sobre esse princípio:

> Essas duas características podem parecer contraditórias, mas a peça que falta no quebra-cabeça é o
> escopo. Quando falamos em ser aberto, estamos falando do design ou da estrutura do software. Dessa
> perspectiva, ser aberto significa que é fácil adicionar novos pacotes, novas interfaces ou novas
> implementações de uma interface existente.

Como bem sublinha Mariano Anaya, o princípio open/closed é aplicável a diversas abstrações de software,
podendo ser uma classe, módulo, função, etc, a ideia é a mesma.

## <a name="2"></a>Como aplicar o open/closed principle em Python?

<!-- TODO -->

## <a name="3"></a>Como aplicar o open/closed principle em Go?

Vejamos um exemplo do Corey Scott de uma função que formata um output a depender do tipo do arquivo:

```go
func BuildOutput(r http.ResponseWriter, format string, p Person) {
	var err error
	switch format {
	case "csv":
		err = outputCSV(r, p)
	case "json":
		err = outputJSON(r, p)
	}
	if err != nil {
		r.WriteHeader(http.StatusInternalServerError)
		return
	}

	r.WriteHeader(http.StatusOK)
}

// função específica hipotética para csv
func outputCSV(writer io.Writer, person Person) error {
	// TODO: implementar
	return nil
}

// função específica hipotética para json
func outputJSON(writer io.Writer, person Person) error {
	// TODO: implementar
	return nil
}

type Person struct {
	Name  string
	Email string
}
```

Se quisermos trabalhar com outro formato, `xml`, por exemplo, teremos que fazer algumas coisas
nesse código acima:

- Adicionar mais um `case` para cada novo formato;
- Adicionar novas funções de output específicas;
- Os métodos que chamam essa função precisam ser adaptados;
- Novos testes precisarão ser adicionados.

Geralmente, quanto mais coisas precisamos modificar no código para abarcar a mudança, mais difícil
fica de não quebrar algo. Existe uma outra forma de escrever esse código:

```go
func BuildOutput(r http.ResponseWriter, pf PersonFormatter, p Person) {
  err := pf.Format(r, p) // chamamos um método hipotético Format de uma interface PersonFormatter
  if err != nil {
    r.WriteHeader(http.StatusInternalServerError)
    return
  }

  r.WriteHeader(http.StatusOK)
}

// definindo comportamento/contrato através de uma interface
type PersonFormatter interface {
	Format(writer io.Writer, person Person) error
}

type CSVPersonFormatter struct{}

// implementação do Format para csv
func (c *CSVPersonFormatter) Format(writer io.Writer, person Person) error {
	// TODO: implement
	return nil
}

type JSONPersonFormatter struct{}

// implementação do format para JSON
func (j *JSONPersonFormatter) Format(writer io.Writer, person Person) error {
	// TODO: implement
	return nil
}

type Person struct {
	Name  string
	Email string
}
```

No código acima abstraímos o `switch` por uma "caixa preta": definimos um comportamento e quem
quiser implementar esse comportamento basta criar seu código específico, seguindo as "regras" da
interface. No fim das contas, cortamos a necessidade de ficar adicionando uma lista enorme de `switch`
e precisamos alterar um lugar a menos no código. Quanto menos lugares para alterar, melhor.

O Corey Scott menciona um benefício interessante do open/closed principle: ajudar a reduzir o escopo
de novos bugs para apenas o novo código, o que faz bastante sentido já que se adicionarmos um novo
formato, como o `xml`, já sabemos onde precisamos olhar se os testes quebrarem:

```go
...
type XMLPersonFormatter struct{}

func (c *XMLPersonFormatter) Format(writer io.Writer, person Person) error {
	// se passou a quebrar depois do PR que implementa isso aqui, estamos diante de uma implementação
    // safada com bug
	return nil
}
```

---

Flash da Jequiti para lembrar os princípios:

<div align="center">
<img alt="solid" src="../../../assets/images/8/solid.webp"/>
</div>

---

Fontes:

ANAYA, Mariano. [Clean Code in Python](https://www.amazon.com.br/Clean-Code-Python-maintainable-efficient/dp/1800560214).
Packt, 2021.

SCOTT, Corey. [Hands-On Dependency Injection in Go](https://www.amazon.com.br/Hands-Dependency-Injection-Corey-Scott/dp/1789132762).
Packt, 2018.
