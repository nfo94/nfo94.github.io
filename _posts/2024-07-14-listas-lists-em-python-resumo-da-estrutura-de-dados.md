---
title: "Listas (lists) em Python: resumo da estrutura de dados"
layout: post
date: 2024-07-14
tags: python list lista
description: resumo sobre a estrutura de dados lista (list) em Python
published: false
---

Este artigo faz um resumo sobre a estrutura de dados lista (list) com exemplos na linguagem Python,
usando o livro [Fluent Python](https://www.amazon.com.br/Fluent-Python-English-Luciano-Ramalho-ebook/dp/B09WZJMMJP/ref=tmm_kin_swatch_0?_encoding=UTF8&dib_tag=se&dib=eyJ2IjoiMSJ9.udVJAGRmoKtaJ2jA7XqGd8dFLuWemRvi2w14-abt4VRCPK5Z_P0NcHJUwkmxdO_w2KE1nYdeOsVCn3EWm-TX0YL4muU8Lqk_qYr6CWpG7VK5-uME-_kMUre5zmdfuDgckcA2szZ077w7Qj35NwxtEM1BvQ0z4-bV8ZCVyoAYsaJ1ff0JPLeGAvhYZ-WMUL6PsLqYjmq_TkKyqrUqAs4j6_6Taw5bAO9iwAjAsaBWPFNaSEMn2CpcWV49tjW7dPrpzyDjg_s2aUF05PBvwDEntSXCoSkQms10Den2q1QgLzc.LBjaiWQ4nyS7F1zhkmQzWqThbh4nLg2gUQm3JuYiQZ4&qid=1721036564&sr=8-1)
como guia. Abordarei aqui:

- [O que é uma lista (list)?](#1)
- [](#2)

## <a name="1"></a>O que é uma lista (list)?

Segundo [Luciano Ramalho](https://github.com/ramalho), **uma lista é um contêiner para sequências,
incluindo contêineres aninhados, e mutável**. Um contêiner para sequências guarda uma referência
para o objeto que contém, que pode ser de qualquer tipo. Exemplo de lista:

```python
codes = [36, 162, 163, 165, 8364, 164]
```

Podemos ser explícitos na criação de uma lista e usar o [list comprehension](https://www.w3schools.com/python/python_lists_comprehension.asp):

```python
fruits = ["apple", "banana", "cherry", "kiwi", "mango"]

newlist = [x for x in fruits if "a" in x]

print(newlist)
```

O código acima gera:

```bash
['apple', 'banana', 'mango']
```

Como o próprio Luciano Ramalho aponta, não escreva "list incomprehensible". Quanto mais linhas sua
list comprehensible tiver mais difícil vai ser para entender; facilite o review dos seus colegas de
trabalho.

## <a name="2"></a>

---

Fontes:

RAMALHO, Luciano. [Fluent Python](https://www.amazon.com.br/Fluent-Python-English-Luciano-Ramalho-ebook/dp/B09WZJMMJP/ref=tmm_kin_swatch_0?_encoding=UTF8&dib_tag=se&dib=eyJ2IjoiMSJ9.udVJAGRmoKtaJ2jA7XqGd8dFLuWemRvi2w14-abt4VRCPK5Z_P0NcHJUwkmxdO_w2KE1nYdeOsVCn3EWm-TX0YL4muU8Lqk_qYr6CWpG7VK5-uME-_kMUre5zmdfuDgckcA2szZ077w7Qj35NwxtEM1BvQ0z4-bV8ZCVyoAYsaJ1ff0JPLeGAvhYZ-WMUL6PsLqYjmq_TkKyqrUqAs4j6_6Taw5bAO9iwAjAsaBWPFNaSEMn2CpcWV49tjW7dPrpzyDjg_s2aUF05PBvwDEntSXCoSkQms10Den2q1QgLzc.LBjaiWQ4nyS7F1zhkmQzWqThbh4nLg2gUQm3JuYiQZ4&qid=1721036564&sr=8-1). O'Reilly, 2022.
