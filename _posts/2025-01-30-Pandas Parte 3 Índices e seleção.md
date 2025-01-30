---
title: "Desbravando o Pandas - Parte 3 - Índices e seleção"
description: Continuando com Pandas, desta vez mostrando o básico de Índices e como seleciona-los.
date: 2025-01-30
categories: [Python, Pandas]
tags: [jupyter notebook, manipulação de dados, engenharia de dados]
image: 
  path: /assets/images/30-01-2025/Capa-30-01-2025.png 
---

Neste documento continuaremos explorarando os conceitos iniciais da biblioteca Pandas. Para acompanhamento utilizaremos a mesma base do [🔗Desbravando o Pandas - Parte 1 - Estrutura de dados](https://lucas-sanbar.github.io/posts/Pandas-Parte-1-Estrutura-de-Dados/) e seguirei com as mesmas estruturas criadas previamente.

---

## Índices

Toda estrutura do tipo DataFrame possui índices, os índices no pandas são essenciais para organizar e acessar dados dentro de um DataFrame ou Series. Eles funcionam como identificadores únicos para as linhas, facilitando buscas, filtragens e operações avançadas.

Existem três tipos de índices, sendo eles: 
* Índices númericos.
* Índices textuais(Labels).
* Índices multi-nível(MultiIndex).

### DataFrame com índices númericos

Normalmente os índices são do tipo **númerico** por padrão, do valor **0** ao **número de linhas do DataFrame - 1**, mas também pode ser do tipo **textual(rótuos/labels)**.

```python
df_data
```

![Bloco 1](/assets/images/30-01-2025/Bloco 1.png)
_Índices criado por padrão no DataFrame `df_data`_

Observe que o índice do DataFrame `df_data` não possui uma coluna especifica para ele. Para recuperar o index no Pandas, basta utilizar o método `index`:

```python
df_data.index
```

![Bloco 2](/assets/images/30-01-2025/Bloco 2.png)
_Índices no DataFrame `df_data`, com ínicio no 0 e fim em 105018_

### DataFrame com índices textuais (Labels)

Um **índice textual** no pandas é quando usamos valores de texto (strings) como índice em um DataFrame ou Series. Isso facilita buscas e manipulações, tornando os dados mais legíveis e organizados.

```python
satisfacao_labels = pd.DataFrame({
    'bom': [50,21,100],
    'ruim': [131,2,30],
    'pessimo': [30,20,1]
}, index = ['Ford','volkswagen', 'BMW'])

satisfacao_labels
```

![Bloco 3](/assets/images/30-01-2025/Bloco 3.png)
_Retorno do DataFrame criado `satisfacao_labels`, com índíce textual_

```python
satisfacao_labels.index
```
![Bloco 4](/assets/images/30-01-2025/Bloco 4.png)
_Índices no DataFrame `satisfacao_labels`, contendo **'Ford'**, **'volkswagen'** e **'BMW'**_

### DataFrame com índices multi-nível (MultiIndex)

O MultiIndex no pandas permite criar índices hierárquicos, ou seja, um índice composto por múltiplas colunas. Isso é útil para organizar e acessar dados estruturados de forma mais eficiente.

Podemos definir múltiplas colunas como índice usando `set_index()`:

```python
MultiIndex = pd.DataFrame({
    'categoria': ['A', 'A', 'B', 'B'],
    'produto': ['P1', 'P2', 'P1', 'P2'],
    'vendas': [100, 150, 200, 250]
})

MultiIndex.set_index(['categoria', 'produto'], inplace=True)

MultiIndex
```

![Bloco 5](/assets/images/30-01-2025/Bloco 5.png)
_Retorno do DataFrame `MultiIndex` com índice multi-nível_

```python
MultiIndex.index
```

![Bloco 6](/assets/images/30-01-2025/Bloco 6.png)
_Índices no DataFrame `MultiIndex`, contendo dois índices, **categoria** e **produto**_

Para resetar o índex ao padrão original, basta usar `MultiIndex.reset_index(inplace=True)`

---

## Seleção de Índex

A seleção de índice nos permite acessar, organizar e manipular os dados de um DataFrame. O índice funciona como um identificador único para as linhas, possibilitando assim buscas rápidas, segmentação e operações estruturadas.

### Seleção baseada em índices

Para mostrar linhas especificas de um DataFrame, usa-se:

`iloc` : Seleciona elementos de um DataFrame, baseando-se em **índices númericos**

#### Selecionando uma amostra/linha

```python
df_data.iloc[1] ## Extraíndo a linha com o índice 1
```

![Bloco 7](/assets/images/30-01-2025/Bloco 7.png)
_Índice 1 no DataFrame `df_data`_

```python
df_data.iloc[1,6] ## Extraíndo a linha com índice 1 e coluna 6 (Faixa Etária)
```

![Bloco 8](/assets/images/30-01-2025/Bloco 8.png)
_Índice 1 e coluna 6 no DataFrame `df_data`_

#### Selecionando **múltiplas** amostras/linhas

Uma das formas de se fazer isso é utilizando **slicing**, passando um intervalo dos nossos valores

```python
df_data.iloc[:3] ## Extraíndo as linhas com índice >= 0 e <= 2
```

![Bloco 9](/assets/images/30-01-2025/Bloco 9.png)
_Retorno da extração das linhas 0,1 e 2 do DataFrame `df_data`_

```python
df_data.iloc[10:13] ## Extraíndo as linhas com índice >= 10 e <= 12
```

![Bloco 10](/assets/images/30-01-2025/Bloco 10.png)
_Retorno da extração das linhas 10,11 e 12 do DataFrame `df_data`_

```python
df_data.iloc[1,5,10] ## Extraíndo as linhas com índice 1,5 e 10
```

![Bloco 11](/assets/images/30-01-2025/Bloco 11.png)
_Retorno da extração das linhas 1,5 e 10 do DataFrame `df_data`_

### Seleção baseada em label

Para seleccionar elementos do DataFrame pelo seu **rótulo/label** usa se:

`loc` : Seleciona elementos de um DataFrame, baseando-se em **rótulo**.

```python
satisfacao_labels.loc['Ford'] ## Retorna a linha cujo o rótulo do índice é 'Ford'
```

![Bloco 12](/assets/images/30-01-2025/Bloco 12.png)
_Retorno da extração do label 'Ford' do DataFrame `satisfacao_labels`_

Semelhante ao `iloc` podemos selecionar linha e coluna baseando no label

```python
satisfacao_labels.loc['Ford','ruim']
```

![Bloco 13](/assets/images/30-01-2025/Bloco 13.png)
_Retorno da extração do label 'Ford' da coluna 'ruim' do DataFrame `satisfacao_labels`_

Se quisermos buscar apenas as colunas **'ruim'** e **'pessimo'** do DataFrame `satisfacao_labels`, existem duas possibilidades:

```python
## Forma 1:
satisfacao_labels[['ruim','pessimo']] ## Retorna todas as linhas do DataFrame mas apenas as colunas 'ruim' e 'pessimo'
```

![Bloco 14](/assets/images/30-01-2025/Bloco 14.png)
_Retorno da extração de todas as linhas e apenas as colunas 'ruim' e 'pessimo' do DataFrame `satisfacao_labels`_

```python
## Forma 2
satisfacao_labels.loc[:,['ruim','pessimo']] ## Retorna os índices 0 a n-1 (:) mas apenas as colunas 'ruim' e 'pessimo'
```

![Bloco 15](/assets/images/30-01-2025/Bloco 15.png)
_Retorno da extração de todas as linhas e apenas as colunas 'ruim' e 'pessimo' do DataFrame `satisfacao_labels`_

Mesmo em DataFrame com índices baseados em rótulos, é possível a utilização do `iloc` para selecionar elementos baseando-se em seu índice implícito, por exemplo, se quisermos seleccionar a linha 2 do DataFrame `satisfacao_labels` basta:

```python
satisfacao_labels.iloc[2] ## Retorna a linha de índice 2 (BMW) do DataFrame
```

![Bloco 16](/assets/images/30-01-2025/Bloco 16.png)
_Retorno da extração da linha de índice 2 (BMW)do DataFrame satisfacao_labels_

>Índices números, por default, possuem rótulos que correspondem aos seus valores númericos. Ou seja, é possível usar o `iloc` e `loc` para índices númericos.
{: .prompt-tip }

#### Seleção de uma ou mais colunas

Já vimos que para selecionar uma coluna de um DataFrame(`df_data`) basta utilizar `df_data['Cidade']` , `df_data.Cidade` (Este, apenas para casos com colunas sem caracteres especiais e sem espaço) ou `data.loc[:, 'Cidade']` .

Para retorno de mais de uma coluna é necessário passar uma lista **['Colune 1','Coluna 2','Coluna 3', '...']**

```python
df_data[['Cidade','UF','Região']] ## Retorna a coluna 'Cidade', 'UF' e 'Região'
```

![Bloco 17](/assets/images/30-01-2025/Bloco 17.png)
_Retorno da extração de todas as linha das colunas 'Cidade', 'UF' e 'Região'_

### Seleção de índices MultiIndex

Para identificar os níveis do índex do tipo MultiIndex, utilizaremos o método `index.levels`

```python
MultiIndex
```

![Bloco 18](/assets/images/30-01-2025/Bloco 18.png)
_Retorno do DataFrame `MultiIndex`_

```python
MultiIndex.index.levels
```

![Bloco 19](/assets/images/30-01-2025/Bloco 19.png)
_Retorno dos níveis do DataFrame `MultiIndex`_

Para identificar os valores do nível 1 e 2 respectivamente, usamos o método `get_level_values(n)`, com **n** sendo o level do índice.

```python
MultiIndex.index.get_level_values(0)  ## Retorna os valores do primeiro nível Categoria
```

![Bloco 20](/assets/images/30-01-2025/Bloco 20.png)
_Retorno dos nível de **Categoria** do DataFrame `MultiIndex`_

```python
MultiIndex.index.get_level_values(1)  ## Retorna os valores do segundo nível Produto
```

![Bloco 21](/assets/images/30-01-2025/Bloco 21.png)
_Retorno dos nível de **Produto** do DataFrame `MultiIndex`_

Para extrair informações de índices MultiIndex devemos passar os n índices requeridos através do método `loc`

```python
MultiIndex.loc[('A', 'P1')]  ## Retorna o valor referente ao índice 1 (Categoria) textual 'A' e ao índice 2 (Produto) textual 'P1'
```

![Bloco 22](/assets/images/30-01-2025/Bloco 22.png)
_Retorno da busca referente ao índice 1 (Categoria) textual 'A' e ao índice 2 (Produto) textual 'P1' do DataFrame `MultiIndex`_

```python
MultiIndex.loc[(['A', 'B'], ['P1']), :] ## Retorna o valor referente ao índice 1 (Categoria) textual 'A' e 'B' e ao índice 2 (Produto) textual 'P1'
```

![Bloco 23](/assets/images/30-01-2025/Bloco 23.png)
_Retorno referente ao índice 1 (Categoria) textual 'A' e 'B' e ao índice 2 (Produto) textual 'P1 do DataFrame `MultiIndex`_

1. `(['A', 'B'], ['P1'])` :
   * O primeiro nível (categoria) recebe uma lista ['A', 'B'], garantindo que ambas sejam selecionadas.
   * O segundo nível (produto) também recebe uma lista ['P1'], garantindo que apenas P1 seja selecionado.
2. Adição de `:` (dois pontos) :
   * Necessário para indicar que queremos todas as colunas.

---

## Resumo dos métodos

|       Método       |                Descrição                 |
| :----------------: | :--------------------------------------: |
|       .index       |         Retornar o objeto índice         |
| .set_index(coluna) |      Define uma coluna como índice       |
|   .reset_index()   | Remove o índice e transforma-o em coluna |
|    .loc[indice]    |    Acessa dados pelo valor do índice     |
|   .iloc[posição]   |    Acessa dados pela posição numérica    |
|   .sort_index()    |      Ordena o DataFrame pelo índice      |
| .droplevel(nível)  | Remove um nível de um índice multi-nível |

---

>Para download do notebook utilizado, acesse o [🔗Link](https://github.com/Lucas-SanBar/PyArq/blob/a7003fc5268c42739287e548fb104e7af59702e2/Desbravando%20Pandas/Parte%203%20-%20%C3%8Dndices%20e%20sele%C3%A7%C3%A3o.ipynb)
{: .prompt-warning }


