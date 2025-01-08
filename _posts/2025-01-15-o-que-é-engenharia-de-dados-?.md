---
title: "O que é engenharia de dados?"
layout: post
date: 2025-01-15
tags: engenharia de dados, etl, banco de dados
description: descrevendo e explicando o que é engenharia de dados e seu ciclo de vida
published: true
---

<figure align="center">
  <img width="100%" src="../../../assets/images/7/big-3520096_1280.webp" alt="imagem de notebook 
  escrito 'big data' de Gerd Altmann no Pixabay">
  <figcaption><i>Imagem de <a href="https://pixabay.com/pt/users/geralt-9301/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=3520096">Gerd Altmann</a> por <a href="https://pixabay.com/pt//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=3520096">Pixabay</a></i></figcaption>
</figure>

O propósito desse texto é descrever brevemente o que é engenharia de dados e seu ciclo
de vida utilizando o livro [Fundamentos de Engenharia de Dados](https://www.amazon.com.br/Fundamentos-Engenharia-Dados-Construa-Sistemas/dp/8575228765/ref=sr_1_1?crid=2XWCTM1GCFZIV&dib=eyJ2IjoiMSJ9.RUj0CiiEh8_elypAdWYI92YmVFpvFJPjoiNLwcPl07Q_YCQvHMZtW7OhPOjNDFfYCR-tvmub57o0OU-_sAI53NNNa650UwWf3zmABlpbiXJIFhGWYPC4LbWMIjXy8AN9W8RlLJcuNSb84OZeAfh-YOkl9GVaGYrd6IJr3_hx-esKdbr6T43S6V6Sq4_XfUvtPWXJS_0PQL4k95QTdtgprzQCYIrNri7Kw6YQn8e5JVRLteoLPsc5s37_BRX39_g6BZQSQxsI5MMHG7UcIcfrrRQ0okhiRtdGeQRE4UDmwqD5EB8Yr0av8hcevapvbTRwXFGFeZqQyEx89jiYo90pyFt3oRwJHMiAPZe1HlV65VIEToIAqiUsF8DS341G11lxqQIf2JGOd2tF187eFkJo7gCfR-qiigOQBpeGc_03a7zkHrfW19w3lH7jRijkUOh3.eNOhhfMg0umQ5cp57TuD3xZFDKg-pjGY6mnQUfOTVH8&dib_tag=se&keywords=fundamentos+de+engenharia+de+dados&qid=1735996483&sprefix=fundamentos+de+enge%2Caps%2C189&sr=8-1&ufe=app_do%3Aamzn1.fos.6d798eae-cadf-45de-946a-f477d47705b9)
do Joe Reis e Matt Housley como base.

- [O que é engenharia de dados?](#1)
- [O ciclo de vida de engenharia de dados](#2)

## <a name="1"></a>O que é engenharia de dados?

Engenharia de dados não é exatamente uma coisa nova e já existia de alguma forma em
empresas, mas tomou mais foco e cresceu mais ou menos no mesmo período em que big data e ciências de dados começaram a aparecer nos holofotes da bolha tech.

De forma resumida, podemos dizer que pessoas engenheiras de dados configuram e operam
a infraestrutura de dados de um empresa, preparando esses dados para analistas de dados,
cientistas de dados, etc. Segundo Reis e Housley:

> **Engenharia de dados é o desenvolvimento, implementação e manutenção de sistemas e
> processos que recebem dados brutos e produzem informações consistentes e de alta
> qualidade que dão suporte a casos de uso posteriores, como análise e aprendizado de
> máquina**. Engenharia de dados é a interseção de segurança, gerenciamento de dados,
> DataOps, arquitetura de dados, orquestração e engenharia de software. Um engenheiro
> de dados gerencia o ciclo de vida da engenharia de dados, começando com a obtenção de
> dados de sistemas de origem e terminando com o fornecimento de dados para casos de
> uso, como análise ou aprendizado de máquina

Ou seja, engenharia de dados foca em receber ou extrair dados crus e trabalhar esses dados
para que possam ser repassados para vários casos de usos. Considerando toda a infrastrutura
que isso abarca, é muito comum que as pessoas profissionais da área tenham um background
em engenharia de software, já que ferramentas como Python, SQL, Java, Scala, bash, entre
outros, são bastante utilizadas aqui.

## <a name="2"></a>O ciclo de vida de engenharia de dados

Há uma imagem no livro [Fundamentos de Engenharia de Dados](https://www.amazon.com.br/Fundamentos-Engenharia-Dados-Construa-Sistemas/dp/8575228765/ref=sr_1_1?crid=2XWCTM1GCFZIV&dib=eyJ2IjoiMSJ9.RUj0CiiEh8_elypAdWYI92YmVFpvFJPjoiNLwcPl07Q_YCQvHMZtW7OhPOjNDFfYCR-tvmub57o0OU-_sAI53NNNa650UwWf3zmABlpbiXJIFhGWYPC4LbWMIjXy8AN9W8RlLJcuNSb84OZeAfh-YOkl9GVaGYrd6IJr3_hx-esKdbr6T43S6V6Sq4_XfUvtPWXJS_0PQL4k95QTdtgprzQCYIrNri7Kw6YQn8e5JVRLteoLPsc5s37_BRX39_g6BZQSQxsI5MMHG7UcIcfrrRQ0okhiRtdGeQRE4UDmwqD5EB8Yr0av8hcevapvbTRwXFGFeZqQyEx89jiYo90pyFt3oRwJHMiAPZe1HlV65VIEToIAqiUsF8DS341G11lxqQIf2JGOd2tF187eFkJo7gCfR-qiigOQBpeGc_03a7zkHrfW19w3lH7jRijkUOh3.eNOhhfMg0umQ5cp57TuD3xZFDKg-pjGY6mnQUfOTVH8&dib_tag=se&keywords=fundamentos+de+engenharia+de+dados&qid=1735996483&sprefix=fundamentos+de+enge%2Caps%2C189&sr=8-1&ufe=app_do%3Aamzn1.fos.6d798eae-cadf-45de-946a-f477d47705b9)
que é excelente para visualizar o ciclo de vida de um pipeline de dados:

<div align="center">
<img alt="data engineering lifecycle" src="../../../assets/images/7/data-engineering-lifecycle.webp"/>
</div>

Ou seja, uma pessoa engenheira de dados tem algumas responsabilidades técnicas:

- Geração de dados
- Armazenamento de dados
- Ingestão de dados
- Transformação de dados
- Servir dados para diferentes casos de uso

Os casos de uso, que podem ser vistos como _downstream_ de dados servidos por um pipeline
desses, incluem análise de dados, machine learning, ciência de dados, reverse ETL
(engenharia reversa de ETL), etc.

---

Fontes:

[Fundamentos de Engenharia de Dados](https://www.amazon.com.br/Fundamentos-Engenharia-Dados-Construa-Sistemas/dp/8575228765/ref=sr_1_1?crid=2XWCTM1GCFZIV&dib=eyJ2IjoiMSJ9.RUj0CiiEh8_elypAdWYI92YmVFpvFJPjoiNLwcPl07Q_YCQvHMZtW7OhPOjNDFfYCR-tvmub57o0OU-_sAI53NNNa650UwWf3zmABlpbiXJIFhGWYPC4LbWMIjXy8AN9W8RlLJcuNSb84OZeAfh-YOkl9GVaGYrd6IJr3_hx-esKdbr6T43S6V6Sq4_XfUvtPWXJS_0PQL4k95QTdtgprzQCYIrNri7Kw6YQn8e5JVRLteoLPsc5s37_BRX39_g6BZQSQxsI5MMHG7UcIcfrrRQ0okhiRtdGeQRE4UDmwqD5EB8Yr0av8hcevapvbTRwXFGFeZqQyEx89jiYo90pyFt3oRwJHMiAPZe1HlV65VIEToIAqiUsF8DS341G11lxqQIf2JGOd2tF187eFkJo7gCfR-qiigOQBpeGc_03a7zkHrfW19w3lH7jRijkUOh3.eNOhhfMg0umQ5cp57TuD3xZFDKg-pjGY6mnQUfOTVH8&dib_tag=se&keywords=fundamentos+de+engenharia+de+dados&qid=1735996483&sprefix=fundamentos+de+enge%2Caps%2C189&sr=8-1&ufe=app_do%3Aamzn1.fos.6d798eae-cadf-45de-946a-f477d47705b9)
