---
title: "Desbravando o Pandas - Parte 4 - Filtragem"
description: Neste post mostrarei o que talvez seja uma das partes mais utilizadas no tratamento de dados, a FILTRAGEM.
date: 2025-02-01
categories: [Python, Pandas]
tags: [jupyter notebook, manipulação de dados, engenharia de dados]
image: 
  path: /assets/images/01-02-2025/Capa-01-02-2025.png 
---

Neste documento continuaremos explorarando os conceitos iniciais da biblioteca Pandas. Para acompanhamento utilizaremos a mesma base do [🔗Desbravando o Pandas - Parte 1 - Estrutura de dados](https://lucas-sanbar.github.io/posts/Pandas-Parte-1-Estrutura-de-Dados/) e seguirei com as mesmas estruturas criadas previamente.

---

## Filtragem

Durante a análise exploratória dos dados, frequentemente será necessário filtrar amostras, a partir de certas condições, para fins de análise mais específica. Existe algumas maneiras de fazermos isso como as mostradas a seguir:

### Filtragem condicional direta

Supondo que nosso objetivo seja trazer apenas as reclamações do site "ConsumidorGov" do estádo **GOIÁS**, como seria feito?

Para fazer essa selação condicional, é necessário buscar o descritivo que representa **SÃO PAULO** e como esta escrita dentro da massa de dados, neste caso utilizaremos a seleção da coluna `UF` juntamente com o método `unique()`, este é responsável por trazer uma array com os campos **distintos** para cada valor do campo selecionado.

```python
## Mostra todos os estados com reclamações no site "Consumidor Gov".
## Mostra os únicos (unique) valores para a coluna 'UF'.
df_data['UF'].unique()
```

![Bloco 1](/assets/images/01-02-2025/Bloco 1.png)
_Valores únicos da coluna **'UF'** do DataFrame `df_data`_

Podemos observar que o nosso objetivo então é fazer a filtragem condicional/direta utilizando o valor 'SP' da coluna `UF`. Podemos fazer isto da seguinte forma:

```python
## Comparação da coluna 'UF' com o valor 'SP'
df_data['UF'] == 'SP'
```

![Bloco 2](/assets/images/01-02-2025/Bloco 2.png)
_Series retornada da filtragem da coluna **'UF'**_

Repare que o retorno é feito __**linha a linha**__ através de uma Series('array') de **booleans** mostrando se a linha em questão respeita(**TRUE**) ou não respeita(**FALSE**) a condição proposta `UF = 'SP'`.

Como filtrar então o DataFrame `df_data` para obter apenas as linhas que contém o campo `UF` igual a 'SP'?

```python
## Armazenando a series retornada em uma variável
selecao = df_data['UF'] == 'SP' 
selecao
```

![Bloco 3](/assets/images/01-02-2025/Bloco 3.png)
_Series retornada da filtragem da coluna **'UF'** armazenada na variável `selecao`_

```python
## Aplicando a filtragem em todos os registros do DataFrame 'df_data' utilizando a series de booleano 'selecao' 
df_data[selecao] 
```

![Bloco 4](/assets/images/01-02-2025/Bloco 4.png)
_Valor **ÚNICO** retornado após a Series `selecao` ser aplicada na filtragem do DataFrane `df_data`_

Desta forma, o retorno do DataFrame é apenas com as linhas onde o campo `UF` é igual a **'SP'**

>Também é possível utilizando o comando `.loc[selecao]` para obter o mesmo resultado.
{: .prompt-tip }

### Filtragem com o método query()

Também é possível utilizar o método `query`, este filtra linhas de um DataFrame baseando-se em uma **pergunta(Query)**.

```python
df_data.query('UF == "SP"')
```

![Bloco 5](/assets/images/01-02-2025/Bloco 5.png)
_Valor **ÚNICO** retornado após a Series `selecao` ser aplicada na filtragem do DataFrane `df_data` utilizando o método `query()`_

Uma boa prática é sempre salvar o DataFrame em uma nova variável. Isso simplifica a complexidade do código para futuras análises feitas.

```python
consumidor_sp = df_data.query('UF == "SP"')
consumidor_sp
```

![Bloco 5](/assets/images/01-02-2025/Bloco 5.png)
_Valor **ÚNICO** retornado após a Series `selecao` ser aplicada na filtragem do DataFrane `df_data` utilizando o método `query()`_

Note, que os índices das linhas mesmo após a filtragem continuam sendo os mesmos do DataFrame `df_data`. Em algumas situações, manter esta informação é importante.

![Bloco 6](/assets/images/01-02-2025/Bloco 6.png)
_Novo DataFrame `consumidor_sp` contém o index das linhas do DataFrame original `df_data` mantidos_

Caso seja necessário *resetar os índices* do Data Frame novo filtrado, use o método `reset_index`.

```python
consumidor_sp.reset_index()
```

![Bloco 7](/assets/images/01-02-2025/Bloco 7.png)
_Novo DataFrame `consumidor_sp` com o Index resetado variando entre 0 até n° linhas - 1_

Note que o retorno mantém o antigo **INDEX** como coluna, caso você queira remover este do novo DataFrame utilize o parâmetro `drop=True` no método `reset_index()`

```python
consumidor_sp.reset_index(drop = True).head()
```

![Bloco 8](/assets/images/01-02-2025/Bloco 8.png)
_Novo DataFrame `consumidor_sp` com o Index resetado variando entre 0 até n° linhas - 1 e a coluna preservada de Index **removida**_

>**OBSEVAÇÃO :** O DataFrame `consumidor_sp` não foi alterado, caso seja necessário alterar é preciso utilizar o parâmetro `inplace=True`.
{: .prompt-tip }
