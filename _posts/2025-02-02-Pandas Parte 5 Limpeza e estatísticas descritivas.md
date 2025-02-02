---
title: "Desbravando o Pandas - Parte 5 - Limpeza e estatísticas descritivas"
description: Neste post, exploraremos as técnicas básicas de limpeza de dados, como tratamento de valores ausentes, correção de dados inconsistentes e a transformação de tipos de dados.
date: 2025-02-02
categories: [Python, Pandas]
tags: [jupyter notebook, manipulação de dados, engenharia de dados]
image: 
  path: /assets/images/02-02-2025/Capa-02-02-2025.png 
---

Neste documento continuaremos explorarando os conceitos iniciais da biblioteca Pandas. Para acompanhamento utilizaremos a mesma base do [🔗Desbravando o Pandas - Parte 1 - Estrutura de dados](https://lucas-sanbar.github.io/posts/Pandas-Parte-1-Estrutura-de-Dados/) e seguirei com as mesmas estruturas criadas previamente.

---

## Limpeza

A limpeza de dados é uma etapa essencial no processo de análise de dados, garantindo que as informações utilizadas sejam precisas, consistentes e utilizáveis. Dados brutos frequentemente contêm erros, valores ausentes, duplicados ou inconsistências que podem distorcer os resultados de qualquer análise. Portanto, a limpeza não apenas melhora a qualidade dos dados, mas também otimiza a eficiência dos modelos preditivos e das decisões baseadas em dados. Neste post, exploraremos as técnicas básicas de limpeza de dados, como tratamento de valores ausentes, correção de dados inconsistentes e a transformação de tipos de dados.

Olhando mais de perto nosso DataFrame `df_data` temos que, ele contém 105.018 linhas e 30 colunas

```python
df_data.shape
```

![Bloco 1](/assets/images/02-02-2025/Bloco 1.png)
_Número de linhas e colunas do DataFrame `df_data`_

Dos quais: 
* Existem colunas contendo valores nulos (ex: **'Nota do Consumidor'**);
* Existem colunas contendo campos com tipo errado (ex: **'Data Abertura'**);
* Existem colunas contendo caracteres especiais (ex: **'Faixa Etária'**), dificultando o uso de certas funções;

```python
df_data.info()
```

![Bloco 2](/assets/images/02-02-2025/Bloco 2.png)
_Informações do DataFrame `df_data`_

Por questão de **COMPARAÇÃO** utilizaremos uma cópia do DataFrame `df_data`:

```python
df_tratado = df_data.copy()
```

### Tratando colunas com dados faltantes (`NaN`)

No nosso DataFrame `df_tratado` existem alguns valores nulos (`NaN`) em diversas colunas. Supondo que a área de negócios impôs a necessidade de tratar estes valores por conta de alguma regra de negócio.

Como isso seria feito?

#### 1. Remover **LINHAS** contendo **QUALQUER** coluna `NaN`

Caso exista essa necessidade, de deixar apenas registros com 100% de preenchimento em todas as colunas. Usa-se o método `dropna()`, este remove todas as linhas que possuem valores nulos de um DataFrame.

```python
df_tratado.dropna().info() ## Remove todas as linhas que possuem valores nulos
```

![Bloco 3](/assets/images/02-02-2025/Bloco 3.png)
_Resultado após a remoção das linhas que contenham ao menos um nulo em alguma coluna do DataFrame `df_tratado`_

Note que após o uso do método `dropna()` apenas 1.208 linhas foram retornadas e não existe mais nenhuma coluna com valores **'Non-Null Count'** diferente de 1.208 (Total de linhas).

#### 2. Remover **COLUNAS** com valores `NaN`

Caso exista essa necessidade, de deixar apenas colunas com 100% de preenchimento em todas as 105.018 linhas. Usa-se o método `dropna()` juntamente com o parâmetro `axis=1`, da seguinte forma `DataFrame.dropna(axis=1)` este remove todas as colunas que contenham ao menos 1 valor `NaN` deixando apenas os campos onde o total de valores **'Non-Null Count'** é igual ao total de linhas no DataFrame.

>O Parâmetro `axis=0|1` indica que a ação em questão será realizado na **LINHA** (`axis=0`) ou na **COLUNA** (`axis=1`).
{: .prompt-tip }

```python
df_tratado.dropna(axis=1).info()
```

![Bloco 4](/assets/images/02-02-2025/Bloco 4.png)
_Resultado após a remoção das colunas que contenham ao menos um nulo em alguma das linhas do DataFrame `df_tratado`_

note que após o uso do método `dropna(axis=1)` apenas 21 colunas restaram. Ou seja, 9 colunas do DataFrame `df_tratado` continha valores do tipo `NaN` em ao menos 1 linha.

#### 3. Preencher valores `NaN`

É possível também substituir valores `NaN` por outro valor desejado.

Por exemplo, existe a necessidade de substituir todos os valores `NaN` pela string **'Sem Valor'**. Podemos cumprir este requisito passando a string desejada dentro do método `fillna()` da seguinte forma:

```python
df_tratado.fillna('Sem Valor').info()
```

![Bloco 5](/assets/images/02-02-2025/Bloco 5.png)
_Resultado após a troca dos valores nulos pela string **'Sem Valor'** do DataFrame `df_tratado`_

Note que não existe mais colunas com o valor **'Non-Null Count'** diferente de 105.018.

Olhando mais de perto:

```python
df_tratado.fillna('Sem Valor').iloc[1] ##Retornando a linha 1 do DataFrame 'df_tratado'
```

![Bloco 6](/assets/images/02-02-2025/Bloco 6.png)
_Detalhado do Index 1 após a troca dos valores nulos pela string **'Sem Valor'** do DataFrame `df_tratado`_

As colunas 'Data Análise', 'Data Recusa', 'Prazo Analise Gestor' e 'Análise da Recusa' que continham `NaN` foram substituidas pela string **'Sem Valor'**

No nosso caso, vamos salvar a substituição feita de `NaN` para outra string mas apenas na coluna **'Análise da Recusa'**:

```python
df_tratado['Análise da Recusa'] = df_tratado['Análise da Recusa'].fillna('Sem Valor')
df_tratado['Análise da Recusa'].unique()
```

![Bloco 7](/assets/images/02-02-2025/Bloco 7.png)
_Resultado após a troca dos valores nulos pela string **'Sem Valor'** da coluna **'Análise da Recusa'** do DataFrame `df_tratado`_

