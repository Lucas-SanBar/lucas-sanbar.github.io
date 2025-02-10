---
title: "Desbravando o Pandas - Parte 7 - Agrupamento e ordenação"
description: Como agrupar e/ou ordenar DataFrames baseando-se em atributos.
date: 2025-02-06
categories: [Python, Pandas]
tags: [jupyter notebook, manipulação de dados, engenharia de dados]
image: 
  path: /assets/images/06-02-2025/Capa-06-02-2025.png 
---

Neste documento continuaremos explorarando os conceitos iniciais da biblioteca Pandas. Para acompanhamento utilizaremos a mesma base do [🔗Desbravando o Pandas - Parte 1 - Estrutura de dados](https://lucas-sanbar.github.io/posts/Pandas-Parte-1-Estrutura-de-Dados/) e seguirei com as mesmas estruturas criadas previamente.

---

## Agrupamento e ordenação

No pandas, os métodos `groupby()` e `sort_values()` são operações fundamentais para organizar e extrair informações. Assim como no SQL as operações de `GROUP BY` e `ORDER BY` estão entre as mais utilizadas.

## Agrupamento

### .groupby()

O agrupamento no Pandas é realizado com o método `groupby()`, que permite segmentar um conjunto de dados com base em uma ou mais colunas. Essa técnica é útil para resumir informações, como calcular médias, somas, contagens e outras estatísticas por categoria. Por exemplo, em um conjunto de dados de vendas, podemos agrupar por categoria de produto e calcular o total vendido para cada uma.

O agrupamento acontece pelo método `groupby()` e ele é semelhante ao **GROUP BY** do SQL.

**Funções** podem então ser aplicadas para os elementos de cada grupo, de modo que os resultados de cada grupo são combinados.

```python
df_grupo = df_data.groupby('Região')

df_grupo
```

![Bloco 1](/assets/images/06-02-2025/Bloco 1.png)
_Objeto de agrupamento criado `df_grupo`_

O groupby não retorna um novo DataFrame, mas sim um **objeto de agrupamento**, que pode ser processado posteriormente com funções como `sum()`, `mean()`, `count()`, entre outras. Esse método é muito útil para agregar dados e identificar padrões em grandes conjuntos de informações.

```python
df_grupo.groups
```

![Bloco 2](/assets/images/06-02-2025/Bloco 2.png)
_Retorno do dicionário gerado utilizando o atributo `groups` no agrupamento `df_grupo`_

O atributo `groups` retorna um dicionário do objeto `groupby()`. Este contém o Index para cada valor do agrupamento. Da seguinte forma:
* As **chaves** são os valores únicos das colunas usadas para agrupar (Neste exemplo a coluna **Região**).
* Os **valores** são listas com os índices das linhas correspondentes a cada grupo.

```python
df_grupo.indices
```

![Bloco 3](/assets/images/06-02-2025/Bloco 3.png)
_Retorno do dicionário gerado utilizando o atributo `indices` no agrupamento `df_grupo`_

O atributo `indices` de um objeto `groupby()` retorna um dicionário, assim como `groups`, mas com uma diferença:
* As **chaves** são os valores únicos das colunas usadas para agrupar (Neste exemplo a coluna **Região**).
* Os **valores** são arrays NumPy contendo os índices das linhas correspondentes a cada grupo.

Se quisermos no nosso tratamento retornar apenas um valor do grupo utilizado, podemos fazer o seguinte:

Supondo que nosso objetivo seja o agrupamento *Região* mas apenas o valor Centro Oeste (**CO**).

```python
## Retornando um DataFrame contendo o grupo do agrupamento utilizando 'CO'
df_grupo.get_group('CO')
```

![Bloco 4](/assets/images/06-02-2025/Bloco 4.png)
_Retorno do DataFrame utilizando o método `get_group()` pegando a 'Região' de 'CO' (Centro Oeste)_

Também é possível utilizar a operação de análise descritiva (Mostrados na parte 5) utilizando também `describe()` e todos atributos individuais.

```python
df_grupo.describe()
```

![Bloco 5](/assets/images/06-02-2025/Bloco 5.png)
_Retorno do DataFrame utilizando o método `describe()` no agrupamento `df_grupo`_

Podemos também agrupar por mais de uma coluna, utilizando o próprio `groupby()` podemos por exemplo:

Qual a média das avaliações das reclamações pra cada **Região** e **Como Comprou Contratou**?

```python
## Agrupa primeiro por 'Região'
## Agrupa segundo por 'Como comprou Contratou'
df_media_regiao = df_data.groupby(['Região','Como Comprou Contratou'])
## Utilizando a função agregadora de média
df_media_regiao['Nota do Consumidor'].mean()
```

![Bloco 6](/assets/images/06-02-2025/Bloco 6.png)
_Retorno do agrupamento `df_media_regiao` utilizando duas colunas 'Região' e 'Como Comprou Contratou' e a média das nótas dos consumidores_

### .agg()

O método `agg()` permite aplicar múltiplas funções de agregação em colunas específicas de um DataFrame ou Series. Ele é muito útil quando queremos aplicar diferentes funções agregadoras a diferentes colunas ao mesmo tempo.

Por exemplo, tendo o seguinte DataFrame:

```python
df = pd.DataFrame([[1, 2, 3],
                   [4, 5, 6],
                   [7, 8, 9],
                   [None, None, None]],
                  columns=['A','B','C'])

df
```

![Bloco 7](/assets/images/06-02-2025/Bloco 7.png)
_Retorno do DataFrame criado `df`_

Podemos aplicar diferentes operações, da seguinte forma:

```python
df.agg([sum, min]) ## Ignora o NaN
```

![Bloco 8](/assets/images/06-02-2025/Bloco 8.png)
_Retorno das funções de agregações `sum` e `min`_

Podemos também utilizar juntamente com a operação `groupby()`

```python
df_grupo['Nota do Consumidor'].agg([min,max])
```

![Bloco 9](/assets/images/06-02-2025/Bloco 9.png)
_Retorno das funções de agregações `min` e `max` com o agrupamento `df_grupo` e o método `agg()`_

---

## Ordenação

A ordenação no Pandas é feita com o método `sort_values()`, que organiza as linhas de um DataFrame com base nos valores de uma ou mais colunas. Podemos ordenar de forma crescente ou decrescente, dependendo da necessidade da análise.

A função básica é : `df.sort_values(by="coluna", ascending=True|False)`
* ascending=True → Define a ordem crescente.
* ascending=False → Define a ordem decrescente.

```python
df_rh = pd.DataFrame({
    "Nome": ["Lucas", "Ana", "Pedro", "Mariana"],
    "Idade": [30, 25, 40, 35],
    "Salario": [4000, 7000, 4000, 8000]
})

df_rh
```

![Bloco 10](/assets/images/06-02-2025/Bloco 10.png)
_DataFrame criado `df_rh`_

Como ordenar o DataFrame `df_rh` pelo salário? 

1. Do **Menor Salário → Maior Salário**:

```python
## Crescente
df_rh.sort_values(by='Salario', ascending=True)
```

![Bloco 11](/assets/images/06-02-2025/Bloco 11.png)
_DataFrame `df_rh` ordenado em ordem crescente **Menor Salário → Maior Salário**_

1. Do **Maior Salário → Menor Salário**:

```python
## Decrescente
df_rh.sort_values(by='Salario', ascending=False)
```

![Bloco 12](/assets/images/06-02-2025/Bloco 12.png)
_DataFrame `df_rh` ordenado em ordem decrescente **Maior Salário → Menor Salário**_

Podemos aplicar o método `sort_values()` por mais de uma coluna.

Se quisermos ordenar de forma **Crescente** o valor do Salário e de forma **Descrescente** a idade. Podemos seguir o seguinte exemplo

```python
## Ordenando o Salário de forma crescente (Menor → Maior)
## Ordenando a Idade de forma decrescente (Maior → Menor)
df_rh.sort_values(by=['Salario','Idade'], ascending=[True,False])
```

![Bloco 13](/assets/images/06-02-2025/Bloco 13.png)
_DataFrame `df_rh` ordenado em ordem crescente **Menor Salário → Maior Salário** pelo Salário e em ordem decrescente **Maior Idade → Menor Idade**_

Note que o DataFrame original não foi alterado:

![Bloco 14](/assets/images/06-02-2025/Bloco 14.png)
_DataFrame `df_rh` não foi alterado_

>Para alterar o DataFrame original `df_rh` é necessário o comando `inplace=True`.
{: .prompt-tip }

Uma abordagem comum é agrupar os dados e, em seguida, ordená-los com base nos valores agregados. Por exemplo, ao analisar um conjunto de dados de vendas, podemos primeiro agrupar por região e calcular o faturamento total, depois ordenar do **maior para o menor**, destacando as regiões com melhor desempenho.

Esses dois métodos são fundamentais para transformar grandes volumes de dados, permitindo análises mais detalhadas e decisões mais informadas

---

>Para download do notebook utilizado, acesse o [🔗Link](https://github.com/Lucas-SanBar/PyArq/blob/7d665e21d89e28b30f6d242ab49770b56f36e752/Desbravando%20Pandas/Parte%207%20-%20Agrupamento%20e%20ordena%C3%A7%C3%A3o.ipynb)
{: .prompt-warning }
