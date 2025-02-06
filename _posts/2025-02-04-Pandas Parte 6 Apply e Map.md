---
title: "Desbravando o Pandas - Parte 6 - Apply e Map"
description: Um resumo sobre mapeamento de funções no Pandas.
date: 2025-02-04
categories: [Python, Pandas]
tags: [jupyter notebook, manipulação de dados, engenharia de dados]
image: 
  path: /assets/images/04-02-2025/Capa-04-02-2025.png 
---

Neste documento continuaremos explorarando os conceitos iniciais da biblioteca Pandas. Para acompanhamento utilizaremos a mesma base do [🔗Desbravando o Pandas - Parte 1 - Estrutura de dados](https://lucas-sanbar.github.io/posts/Pandas-Parte-1-Estrutura-de-Dados/) e seguirei com as mesmas estruturas criadas previamente.

---

## Apply e Map

No pandas, as funções `apply()` e `map()` são amplamente utilizadas para transformar e manipular dados dentro de DataFrames e Series. Ambas permitem a aplicação de funções personalizadas a todos os elementos da estrutura, mas possuem diferenças importantes em sua aplicação.

`apply()` :

* Pode ser usada tanto em Series quanto em DataFrames.
* Em Series, aplica uma função a cada elemento.
* Em DataFrames, permite aplicar funções em colunas (axis=0) ou em linhas (axis=1).
* É útil para transformações complexas que envolvem múltiplas colunas.

`map()` :
* Aplicável apenas a Series.
* Aplica funções elemento por elemento, sendo ideal para mapeamentos simples.
* Suporta dicionários para substituição de valores e funções lambda para transformações diretas.

Em geral, `map()` é mais eficiente quando a operação pode ser feita elemento a elemento, enquanto `apply()` é mais flexível para manipulações que envolvem múltiplas colunas ou estruturas complexas.

|       | DataFrame | Series |
| :---: | :-------: | ------ |
| apply |     ✔️     | ✔️      |
|  map  |     ❌     | ✔️      |

Para teste, utilizaremos o DataFrame criado a seguir `df_produtos`:

```python
df_produtos = pd.DataFrame({
    'Produto': ['Notebook', 'Smartphone', 'Tablet', 'Monitor'],
    'Valor': [4500, 2500, 1500, 1200],
    'Margem': [0.2, 0.15, 0.25, 0.18],
    'Estoque': [100, 250, 150, 200]},
    index=['Produto 1','Produto 2','Produto 3','Produto 4'])

df_produtos
```

![Bloco 1](/assets/images/04-02-2025/Bloco 1.png)
_DataFrame criado `df_produtos`_

### .apply()

`apply()` : É utilizado para aplicar uma função ao longo de algum eixo do DataFrame ou em valores de uma Series.

`axis=0` : Significa que estamos selecionando linhas ↑ ↓

`axis=1`: Significa que estamos selecionando colunas → ←

Vamos criar duas funções para exemplificar a diferença dos eixos:

```python
## Criando função personalizada
def soma_produtos(series):
    return series.sum() # Retorna a soma de todos os produtos dado um determinado eixo
```

```python
## Criando função personalizada
def margem_venda(series):
    return series.iloc[1] * series.iloc[2] ## Retorna a margem de venda de um dos produtos (Valor * Margem)
```

#### `axis=0` - Seleção de linhas

```python
## Aplicando a função personalizada soma_produtos
df_produtos.apply(soma_produtos,axis=0) 
```

![Bloco 2](/assets/images/04-02-2025/Bloco 2.png)
_Retorno da aplicação da função personalizada `soma_produtos`_

Repare que todas as linhas do DataFrame `df_protudos` foram somadas, criando uma nova linha que pode ser inserida de volta no DataFrame.

#### `axis=1` - Seleção colunas

```python
## Aplicando a função personalizada margem_venda e criando uma nova coluna com o valor retornado
df_produtos['Valor Margem'] = df_produtos.apply(margem_venda,axis=1)

df_produtos
```

![Bloco 3](/assets/images/04-02-2025/Bloco 3.png)
_Retorno da aplicação da função personalizada `Valor Margem`_

Repare que a função foi aplicada `.loc[1]` (Valor) * `.loc[2]` (Margem), criando uma nova coluna no DataFrame **Valor Margem**.

#### Funções `lambda`

No pandas, uma função `lambda` é uma função temporária que pode ser usada de forma rápida. 

```python
df_produtos['Ganhos Possíveis'] = df_produtos[['Produto','Valor','Margem','Estoque']].apply(lambda series: (series.loc['Valor'] * series.loc['Margem']) * series.loc['Estoque'],axis=1)

df_produtos
```

![Bloco 4](/assets/images/04-02-2025/Bloco 4.png)
_Retorno da aplicação da função temporária `lambda`_

A função `lambda` é implicitamente utilizada em alguns momentos. Como neste mesmo caso por exemplo:

```python
df_produtos['Ganhos Possíveis 2'] = ( df_produtos['Valor'] * df_produtos['Margem'] ) * df_produtos['Estoque']

df_produtos
```

![Bloco 5](/assets/images/04-02-2025/Bloco 5.png)
_Retorno da aplicação sem a utilização de `lambda`_

Quando devo ou não utilizar a função `lambda` ao invés da criação de uma função personalização `def`?

O uso de funções lambda no pandas pode tornar o código mais conciso e eficiente, mas existem momentos em que é melhor evitá-las.

1. Quando utilizar `lambda`?

* Se a função for curta e não precisar ser reutilizada, `lambda` é uma boa escolha.
* Uso com `apply()` para manipular múltiplas colunas.
* Uso com `map()` para substituições simples.

2. Quando **NÃO** utilizar `lambda`?

* Funções complexas e difíceis de ler.
* Duplicação de código (Se uma cálculo é feito mais do que uma vez, considere `def`).
* Para operações vetorizadas (Como o exemplo anterior):
    * `df['Valor_Dobrado'] = df['Valor'].apply(lambda x: x * 2)` tem um desempenho pior do que `df['Valor_Dobrado'] = df['Valor'] * 2`

De forma resumida:

|                    Situação                    | Usar `lambda` | Alternativa               |
| :--------------------------------------------: | :-----------: | ------------------------- |
|           Operações curtas e diretas           |       ✔️       | -                         |
| Aplicação em colunas individuais com `apply()` |       ✔️       | -                         |
|         Conversão simples com `map()`          |       ✔️       | -                         |
|   Funções longas ou com múltiplas condições    |       ❌       | `def` com `apply()`       |
|        Código precisa ser reutilizável         |       ❌       | Criar uma função separada |
|          Operação pode ser vetorizada          |       ❌       | Métodos nativos do pandas |


### .map()

Diferente da função `apply()` a função `map()` é exclusiva para Series, sendo usada para aplicar uma função a cada elemento de uma Series.

```python
## Criando uma Series nomes
nomes = pd.Series(['Lucas','Andressa','Bruno','Maria'])

nomes
```

![Bloco 6](/assets/images/04-02-2025/Bloco 6.png)
_Retorno do DataFrame criado `nomes`_

Assim como `apply()` podemos utilizar funções personalizadas `def` ou funções pré definidas no Pandas como por exemplo a função `upper()`:

```python
## Aplicando a função Upper via map na Series nomes
nomes.map(lambda x: x.upper())
```

![Bloco 7](/assets/images/04-02-2025/Bloco 7.png)
_Retorno da função `map()` utilizando o método `upper()`_

Lembrando que, o Pandas já fornece uma série de métodos para manipulação de dados e não existe necessidade de utilizar a função `map()` quando estamos falando delas.

Como por exemplo, a própria função `upper()` já existe:

```python
nomes.str.upper()
```

![Bloco 8](/assets/images/04-02-2025/Bloco 8.png)
_Retorno do DataFrame utilizando o método `upper()` sem `map()`_

#### Utilizando dicionários

Além de funções, `map()` também pode ser usado com dicionários de valores mapeados:

```python
## De-Para de nomes
De_Para = {'Lucas': 'Lucas Santos', 'Andressa': 'Andressa Ferraz', 'Bruno': 'Bruno Barbosa', 'Maria': 'Maria Elias'}

De_Para
```

![Bloco 9](/assets/images/04-02-2025/Bloco 9.png)
_Dicionário/De-Para de nomes criado `De-Para`_

>A utilização de dicionário juntamente com o `map()` é semelhante ao `Applymap()` do **QLIK**.
{: .prompt-tip }

```python
nomes = nomes.map(De_Para)

nomes
```

![Bloco 10](/assets/images/04-02-2025/Bloco 10.png)
_Aplicação do dicionário criado `De-Para` utilizando `map()`_

>Como não estão no dicionário/De-Para, o `map()` retorna `NaN` para eles.
{: .prompt-tip }

---

### Resumo

Principais diferenças entre `apply()` e `map()`:

| Situação                          | Usar `apply()`                                                            | Usar `map()`                 |
| --------------------------------- | ------------------------------------------------------------------------- | ---------------------------- |
| Onde pode ser usada?              | Series e DataFrame                                                        | Apenas Series                |
| Forma de aplicação                | Pode ser usada linha por linha (`axis=1`) ou coluna por coluna (`axis=0`) | Aplica elemento por elemento |
| Pode usar dicionário?             | ❌ Não                                                                     | ✅ Sim                        |
| Pode usar funções personalizadas? | ✅ Sim                                                                     | ✅ Sim                        |


---

>Para download do notebook utilizado, acesse o [🔗Link](https://github.com/Lucas-SanBar/PyArq/blob/3487f43a36c4e4b5ce1a1b134344a90ee5ceb5a4/Desbravando%20Pandas/Parte%206%20-%20Apply%20e%20Map.ipynb)
{: .prompt-warning }
