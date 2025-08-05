---
title: "SOLID: open/closed principle em Go e Python"
layout: post
date: 2025-08-11
tags: solid, open/closed principle
description: explicando o que é o open/closed principle do SOLID, com exemplos em Python e em Go
published: true
---

Esse é o segundo artigo de uma série sobre SOLID explicado com exemplos nas linguagens de
programação Python e Go, da forma mais simples posssível, utilizando dois livros como base, um do
Corey Scott e outro do Mariano Anaya:

- [Hands-On Dependency Injection in Go](https://www.amazon.com.br/Hands-Dependency-Injection-Corey-Scott/dp/1789132762)
- [Clean Code in Python](https://www.amazon.com.br/Clean-Code-Python-maintainable-efficient/dp/1800560214)

Para atualizar os exemplos de Python do "Clean Code in Python" utilizaremos também o artigo do [Real Python](https://realpython.com/solid-principles-python/#open-closed-principle-ocp).
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

O Mariano Anaya dá um exemplo interessante em seu livro, acompanhe meus comentários no código dele:

```python
from dataclasses import dataclass

# O decorator `dataclass` dá a classe Python uma série de métodos e atributos básicos built-in
# sem que a gente precise escrevê-los na mão, como um `__init__(self, raw_data: dict)`, por exemplo
@dataclass
class Event:
    """`Event` é uma classe que define um evento"""
    raw_data: dict

# Cada classe abaixo herda do `Event` e especifica um tipo de evento. Imagine uma implementação
# hipotética do que cada uma faz ou tem
class UnknownEvent(Event):
    pass

class LoginEvent(Event):
    pass

class LogoutEvent(Event):
    pass

class SystemMonitor:
    """`SystemMonitor` identifica eventos na aplicação"""

    # Inicializando o `SystemMonitor` com um atributo `event_data`
    def __init__(self, event_data):
        self.event_data = event_data

    # Método para identificar um evento: se é um `LoginEvent`, `LogoutEvent` ou `UnknownEvent`
    def identify_event(self):
        if (
            self.event_data["before"]["session"] == 0
            and self.event_data["after"]["session"] == 1
        ):
            return LoginEvent(self.event_data)
        elif (
            self.event_data["before"]["session"] == 1
            and self.event_data["after"]["session"] == 0
        ):
            return LogoutEvent(self.event_data)

        return UnknownEvent(self.event_data)
```

Agora, imagine que queremos adicionar mais 3 eventos a essa aplicação. Você diria que o `SystemMonitor`
está "aberto para extensão, fechado para modificação"? Pense bem na parte de "modificação". Para extender
o monitoramento, além da inevitável adição de um novo evento (nova subclasse) precisaríamos modificar o
`SystemMonitor` com um novo `elif`. Para conseguir cumprir o princípio precisamos, de alguma forma,
fazer com que a lógica do `SystemMonitor` "seja uma só" e que a "responsabilidade" da nova feature
seja justamente do novo código, do novo evento. Uma forma de fazer isso é como no código abaixo:

<!-- TODO explicar código -->

```python
from dataclasses import dataclass

# A classe `Event` é a nossa classe "genérica"
@dataclass
class Event:
    raw_data: dict

    # Queremos abstrair a identificação do evento, e aqui exemplificamos uma interface comum para
    # todos os eventos seguirem
    @staticmethod
    def meets_condition(event_data: dict):
        return False

class UnknownEvent(Event):
    pass

class LoginEvent(Event):
    # Agora cada nova subclasse de `Event` implementa seu próprio `meets_condition`. Esse método
    # recebe um dicionário `event_data` e computa uma lógica para definir se é o tipo de evento em
    # questão ou não
    @staticmethod
    def meets_condition(event_data: dict):
        return (
            event_data["before"]["session"] == 0
            and event_data["after"]["session"] == 1
        )

class LogoutEvent(Event):
    @staticmethod
    def meets_condition(event_data: dict):
        return (
            event_data["before"]["session"] == 1
            and event_data["after"]["session"] == 0
        )

# Novo evento. Repare em como basta implementar sua lógica, seguindo a "interface" da classe genérica
class TransactionEvent(Event):
    @staticmethod
    def meets_condition(event_data: dict):
        return event_data["after"].get("transaction") is not None

class SystemMonitor:
    def __init__(self, event_data):
        self.event_data = event_data

    def identify_event(self):
        # Agora para identificar o evento usamos o método mágico (dunder/magic method)
        # `__subclasses__()` para "olhar" as subclasses do `Event`
        for event_cls in Event.__subclasses__():
            try:
                # Chamando a implementação de clada subclasse passando o `event_data`, e a partir
                # daí a lógica é responsabilidade de cada evento
                if event_cls.meets_condition(self.event_data):
                    # Se satisfaz a condição para ser o evento, retorne a subclasse com os dados do
                    # evento
                    return event_cls(self.event_data)
            except KeyError:
                continue
        return UnknownEvent(self.event_data)
```

Com o exemplo acima não precisamos ficar alterando o `SystemMonitor` toda vez que um evento novo
for criado, bastará apenas criar uma nova subclasse e implementar a interface da classe genérica
`Event`.

> _Poxa, mas não tem um exemplo mais moderno e mais simples não, sem usar `__subclasses__()`?_

Exatamente o que pensei ao ler o código, e pensei logo no pacote `abc` do Python. Logo em seguida o
autor Mariano Anaya citou a possiblidade de fazer de outras formas, justamente com o `abc`. Vejamos
o exemplo do site Real Python:

```python
from math import pi

class Shape:
    # Inicializamos a class recebendo um `shape_type` e `kwargs`
    def __init__(self, shape_type, **kwargs):
        self.shape_type = shape_type
        # Para definir `height` e `width` vamos de `if` e `elif` até dizer chega
        if self.shape_type == "rectangle":
            self.width = kwargs["width"]
            self.height = kwargs["height"]
        elif self.shape_type == "circle":
            self.radius = kwargs["radius"]

    # Método para calcular a área a depender do tipo de forma
    def calculate_area(self):
        if self.shape_type == "rectangle":
            return self.width * self.height
        elif self.shape_type == "circle":
            return pi * self.radius**2
```

Consegue ver o padrão se repetir aqui? Adicionar mais tipos de formas, de shapes, faz com que tenhamos
que modificar a classe com mais `if`. Podemos utilizar essa classe para criar diferentes formas, e
ela funciona, mas não segue o princípio open/closed. Uma possível solução:

```python
from abc import ABC, abstractmethod
from math import pi

# Finalmente, estamos usando o pacote `abc`! Na nossa classe genérica, `Shape`, também chamada de
# classe abstrata, e herdamos de `ABC`
class Shape(ABC):
    def __init__(self, shape_type):
        self.shape_type = shape_type

    # E aqui criamos um método abstrato com o `abstractmethod`. Quem quiser que implemente 😒💅
    @abstractmethod
    def calculate_area(self):
        pass

# `Circle`, um tipo de `Shape`, herda de `Shape`, nossa classe abstrata
class Circle(Shape):
    def __init__(self, radius):
        # Chamando o construtor da superclasse, passando o tipo de forma
        super().__init__("circle")
        self.radius = radius

    # Implementando o `calculate_area` do `Circle`
    def calculate_area(self):
        return pi * self.radius**2

class Rectangle(Shape):
    def __init__(self, width, height):
        super().__init__("rectangle")
        self.width = width
        self.height = height

    # Implementando o `calculate_area` do `Rectangle` e por aí vai. Quem pegou o bonde andando não
    # senta na janela: implemente sua poha
    def calculate_area(self):
        return self.width * self.height
```

Agora podemos dizer que nosso `Shape` aberto para extensão e fechado para modificação.

## <a name="3"></a>Como aplicar o open/closed principle em Go?

Agora vamos ver como isso funciona em Go. Vejamos um exemplo do autor Corey Scott de uma função que
formata um output a depender do tipo do arquivo:

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

// Função específica hipotética para csv
func outputCSV(writer io.Writer, person Person) error {
	// TODO: implementar
	return nil
}

// Função específica hipotética para json
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
  err := pf.Format(r, p) // Chamamos um método hipotético Format de uma interface PersonFormatter
  if err != nil {
    r.WriteHeader(http.StatusInternalServerError)
    return
  }

  r.WriteHeader(http.StatusOK)
}

// Definindo comportamento/contrato através de uma interface
type PersonFormatter interface {
	Format(writer io.Writer, person Person) error
}

type CSVPersonFormatter struct{}

// Implementação do Format para csv
func (c *CSVPersonFormatter) Format(writer io.Writer, person Person) error {
	// TODO: implement
	return nil
}

type JSONPersonFormatter struct{}

// Implementação do format para JSON
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
	// Se passou a quebrar depois do PR que implementa isso aqui, estamos diante de uma implementação
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

[SOLID Principles: Improve Object-Oriented Design in Python - Real Python](https://realpython.com/solid-principles-python/#open-closed-principle-ocp)
