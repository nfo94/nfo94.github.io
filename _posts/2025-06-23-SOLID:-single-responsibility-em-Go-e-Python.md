---
title: "SOLID: single responsibility em Go e Python"
layout: post
date: 2025-06-23
tags: solid, single responsibility principle
description: explicando o que é o single responsibility principle do SOLID, com exemplos em Python e em Go
published: true
---

Esse é o primeiro artigo de uma série sobre SOLID explicado com exemplos nas linguagens de
programação Python e Go, da forma mais simples posssível, e iremos utilizar dois livros como base,
um do Corey Scott e outro do Mariano Anaya:

- [Hands-On Dependency Injection in Go](https://www.amazon.com.br/Hands-Dependency-Injection-Corey-Scott/dp/1789132762)
- [Clean Code in Python](https://www.amazon.com.br/Clean-Code-Python-maintainable-efficient/dp/1800560214)

Aqui iremos falar sobre o **single responsibility principle**, ou **princípio de responsabilidade
única** em português. Perguntas para guiar nosso estudo:

- [O que é e para que serve o single responsibility principle?](#1)
- [Como aplicar o single responsibility principle em Python?](#2)
- [Como aplicar o single responsibility principle em Go?](#3)

## <a name="1"></a>O que é e para que serve o single responsibility principle?

De todos os princípios SOLID esse é provavelmente o mais simples de entender. A ideia do single
responsibility principle é **reduzir complexidade decompondo o código em partes menores, mais fáceis
de entender, constatando que um componente de software, uma classe, por exemplo, deve ter apenas uma
responsabilidade**.

Esse princípio também está relacionado ao **separation of concerns**, onde temos a ideia de manter a
coesão do código definindo bem quais são as "preocupações", ou responsabilidades, que determinado
componente possui.

Uma outra forma de pensar sobre o single responsibility principle (por vezes abreviado para **SRP**)
é que, caso uma classe não siga esse princípio e apresente várias responsabilidades, isso também
significa que essa classe tem vários motivos para acarretar mudanças. Se tivesse uma única
responsabilidade, caso fosse necessário mudar algo isso seria feito de uma vez só.

Além do já citado, o **SRP possivelmente aumenta a reusabilidade de código** uma vez que as
responsabilidades estão bem separadinhas e desacopladas de um componente específico. E na linha do
desacoplamento até escrever testes é potencialmente mais simples.

## <a name="2"></a>Como aplicar o single responsibility principle em Python?

Vejamos o código extraído do [Clean Code in Python](https://www.amazon.com.br/Clean-Code-Python-maintainable-efficient/dp/1800560214):

```python
class SystemMonitor:
    # Responsabilidade 1
    def load_activity(self):
        pass

    # Responsabilidade 2
    def identify_events(self):
        pass

    # Responsabilidade 3
    def stream_events(self):
        pass
```

A mesma classe `SystemMonitor` é responsável por várias coisas e cada uma dessas coisas pode apresentar
motivos para mudar, considerando seus contextos e fluxo. Exemplo: imagine que precisamos fazer alguma
transformação adicional na hora de fazer stream de eventos; teríamos que adaptar a função `stream_events`,
e assim sucessivamente com cada coisa do "universo" de cada responsabilidade que a class contém.

O ideal seria desacoplar essas múltiplas responsabilidades em outras classes:

<div align="center">
<img alt="single responsability" src="../../../assets/images/8/srp.webp"/>
</div>

O UML acima pode ser interpretado desse forma em Python:

```python
class Event:
    pass

class ActivityWatcher:
    pass

class SystemMonitor:
    pass

class Output:
    pass

class AlertSystem:
    def __init__(self):
        self._activity_watcher = ActivityWatcher()
        self._system_monitor = SystemMonitor()
        self._output_system = Output()
    ...
```

## <a name="3"></a>Como aplicar o single responsibility principle em Go?

Aqui temos um ponto importante: Go não tem classes, mas podemos pensar na aplicação do SRP em Go
em termos de objetos (structs, functions, interfaces ou packages). Utilizando um exemplo do [Hands-On Dependency Injection in Go](https://www.amazon.com.br/Hands-Dependency-Injection-Corey-Scott/dp/1789132762):

```go
type Calculator struct {
	data map[string]float64
}

// Responsabilidade 1
func (c *Calculator) Calculate(path string) error {
}

// Responsabilidade 2
func (c *Calculator) Output(writer io.Writer) {
}
```

As funções acima não estão seguindo o SRP, temos mais de uma responsabilidade sendo implementada
nesse struct. Se quisermos adicionar uma forma de printar o output em csv:

```go
type Calculator struct {
	data map[string]float64
}

// Responsabilidade 1
func (c *Calculator) Calculate(path string) error {
}

// Responsabilidade 2
func (c *Calculator) Output(writer io.Writer) {
}

// Adicionando mais responsabilidade
func (c Calculator) OutputCSV(writer io.Writer) {
}
```

O exemplo abaixo demonstra como desacoplar as várias responsabilidades atreladas ao `Calculator`:

```go
// Objeto para calcular
type Calculator struct {
    data map[string]float64
}

// Calcular é uma responsabilidade
func (c *Calculator) Calculate(path string) error {
}

func (c *Calculator) getData() map[string]float64 {
}

// Comportamento
type Printer interface {
    Output(data map[string]float64)
}

// Objeto para printar
type DefaultPrinter struct {
    Writer io.Writer
}

// Printar é outra responsabilidade, implementando o comportamento Output
func (d *DefaultPrinter) Output(data map[string]float64) {
}

// Objeto para printar em CSV
type CSVPrinter struct {
  Writer io.Writer
}

// Outra responsabilidade, que implementa seu próprio comportamento Output
func (d *CSVPrinter) Output(data map[string]float64) {
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
