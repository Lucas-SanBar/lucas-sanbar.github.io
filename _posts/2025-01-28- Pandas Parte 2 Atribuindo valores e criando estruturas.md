---
title: "Desbravando o Pandas - Parte 2 - Atribuindo valores e criando atributos"
description: A continuação da explorção da biblioteca Pandas, desta vez mostrando como atribuir valores e como criar atributos.
date: 2025-01-28
categories: [Python, Pandas]
tags: [jupyter notebook, manipulação de dados, engenharia de dados]
image: 
  path: /assets/images/28-01-2025/Capa-28-01-2025.png 
---

Neste documento continuaremos explorarando os conceitos iniciais da biblioteca Pandas. Para acompanhamento utilizaremos a mesma base do [🔗Desbravando o Pandas - Parte 1 - Estrutura de dados](https://lucas-sanbar.github.io/posts/Pandas-Parte-1-Estrutura-de-Dados/) e seguirei com as mesmas estruturas criadas previamente.

---

## Atribuindo Dados

Atribuir dados significa modificar ou adicionar valores em um DataFrame ou Series. A forma como a atribuição é feita pode ter impactos significativos no comportamento, como criar cópias ou referências, alterar os dados originais ou gerar erros. Abaixo estão os principais conceitos e práticas.

### Atribuindo Constantes

Ao fazermos uma atribuição de uma coluna de um DataFrame, como por exemplo a coluna `Cidade` no DataFrame `df_data`

```python
df_data['Cidade']
```

![Bloco 1](/assets/images/28-01-2025/Bloco 1.png)
_Valores da coluna **Cidade** do DataFrame `df_data`_

Podemos fazer a atribuição de valores de duas formas, **CÓPIA** e **REFERÊNCIA/VIEW**. Para isso:

```python
cidade_copy = df_data['Cidade'].copy() #Cópia da coluna Cidade
cidade_copy
```

![Bloco 2](/assets/images/28-01-2025/Bloco 2.png)
_Valores da coluna **Cidade** copiados para a variável `cidade_copy`_

```python
cidade_view = df_data['Cidade'] #Referência/View da coluna Cidade
cidade_view
```

![Bloco 3](/assets/images/28-01-2025/Bloco 3.png)
_Valores da coluna **Cidade** referenciados para a variável `cidade_view`_

E qual a diferença entre **REFERÊNCIA/VIEW** e **CÓPIA**?
Caso alguma alteração no DataFrame original `df_data` seja feita, temos que:

Para exemplificar utilizando a função `iloc`, esta nos permite selecionar linhas e colunas no DataFrame com base em uma posição númerica, vamos alterar então a **LINHA 0** e **COLUNA 4**, para isso podemos fazer da seguinte forma:

```python
df_data.iloc[0,4] = 'Alterado' #Alterando o DataFrame original no índice 0 e coluna 4 para 'Alterado'
df_data.head(1) #Retornando apenas o primeiro índice
```
![Bloco 4](/assets/images/28-01-2025/Bloco 4.png)
_Valores do Índice 0 e coluna 4 alterado_

Após a alteração, se buscarmos a **CÓPIA** teremos o seguinte:

```python
cidade_copy
```
![Bloco 5](/assets/images/28-01-2025/Bloco 5.png)
_Valores da variável `cidade_copy` após alteração_

Após a alteração, se buscarmos a **REFERÊNCIA/VIEW** teremos o seguinte:

```python
cidade_view
```
![Bloco 6](/assets/images/28-01-2025/Bloco 6.png)
_Valores da variável `cidade_view` após alteração_

Nota-se então, que os valores da **CÓPIA** se mantém mesmo após a alteração do DataFrame original `df_data`, enquanto a **REFERÊNCIA** por ser um ponteiro ao DataFrame original também tem seu valor alterado.

A escolha entre view (referência direta) e cópia (duplicação dos dados) no pandas depende do contexto em que você está trabalhando e dos objetivos da operação. Cada abordagem tem vantagens e desvantagens e deve ser usada com cuidado para evitar comportamentos inesperados ou problemas de desempenho.

Quando utilizar **Referência/View**?

Uma view é útil quando você quer manipular ou visualizar dados diretamente no DataFrame original sem duplicá-los na memória.

Casos comuns:
* Quando você deseja modificar valores existentes no DataFrame original.
* Quando você busca eficiência de memória, já que não existe a necessidade de duplicar estruturas.
* Usar views é eficiente para acessar partes de um DataFrame que você deseja processar ou analisar diretamente.
* Evitar duplicação acidental.

Quando utilizar **Cópia**?

Uma cópia cria um novo objeto com dados independentes, útil quando você precisa trabalhar com dados isolados do DataFrame original.

Casos comuns:
* Se você deseja processar ou transformar os dados sem impactar o DataFrame original.
* Evitar efeitos colaterais inesperados.
* Comparar antes e depois de alguma alteração.
* Isolamento de processamento paralelo (Em pipelines de dados ou processos paralelos, criar cópias ajuda a evitar conflitos entre threads ou funções).
* Usar uma cópia garante que mudanças feitas por terceiros não afetem sua análise.

Boas práticas

1. **Garanta a Intenção**: Use .copy() explicitamente se não quiser modificar o DataFrame original.
2. **Evite Substituições Desnecessárias**: Modifique valores in-place usando .loc ou .iloc quando possível.
3. **Leia Warnings**: O SettingWithCopyWarning avisa quando o pandas detecta operações ambíguas entre views e cópias.

| Situação                                          | Use View | Use Cópia |
| ------------------------------------------------- | -------- | --------- |
| Modificar dados direto no DataFrame               | ✔️ Sim    | ❌ Não     |
| Manter dados originais intactos                   | ❌ Não    | ✔️ Sim     |
| Evitar uso excessivo de memória                   | ✔️ Sim    | ❌ Não     |
| Comparar dados antes e depois de alguma alteração | ❌ Não    | ✔️ Sim     |
| Trabalhar em subconjunto temporário               | ✔️ Sim    | ❌ Não     |
| Isolamento entre processos e threads              | ❌ Não    | ✔️ Sim     |

### Atribuindo Listas ou Series

O nosso DataFram `df_data` contém **105018 linhas** e **30 colunas**, para fazer a alteração de uma coluna inteira precisamos de uma **Series** contendo o mesmo número de linhas existente na coluna objetivo ou seja, uma lista contendo 105018

```python
n_linhas, n_colum = df_data.shape
n_linhas, n_colum
```

![Bloco 7](/assets/images/28-01-2025/Bloco 7.png)
_Resultado da execução `df_data.shape`, com a dimensão do DataFrame criado `df_data`, contendo 105018 linhas e 30 colunas_

Para criação destes 105018 valores de uma coluna utilizaremos **LIST COMPREHENSION**, que é uma forma de criar listas a partir de outras listas, usando uma notação matemática.

```python
novas_cidades = [f'Cidade {i}'for i in range(n_linhas)]
len(novas_cidades) #Retorna o número de itens de um objeto, como uma lista, string, tupla, dicionário ou qualquer sequência
```

![Bloco 8](/assets/images/28-01-2025/Bloco 8.png)
_Quantidade de valores após criação da lista `novas_cidades`_

>A lista `novas_cidades` recebe um looping de 0 até o número de linhas do DataFrame `df_data`, onde cada passagem a string **'Cidade '** é concatenado com o valor do índice da passagem **i**
{: .prompt-tip }

A quantidade de elementos na lista `novas_cidades` é igual ao número de linhas do DataFrame `df_data`, podemos então fazer a substituição dos valores da coluna Cidade para o novo valor.

```python
df_data['Cidade'] = novas_cidades #Substituindo os valores da coluna Cidade pelos valores da lista novas_cidades
df_data.head()
```

![Bloco 9](/assets/images/28-01-2025/Bloco 9.png)
_Valores trocados após alteração para `novas_cidades`_

Para retornarmos os dados de **Cidade** originais, podemos utilizar a cópia feita na Series `cidade_copy`, esta Series foi criada e copiada antes da alteração da coluna, portanto tem os valores originais da mesma.

```python
df_data['Cidade'] = cidade_copy #Retornando os valores originais da coluna Cidade utilizando a cópia feita antes da alteração
df_data.head()
```

![Bloco 10](/assets/images/28-01-2025/Bloco 10.png)
_Valores retornados para o origal utilizando a cópia feita em `cidade_copy`_

---

## Criando novas colunas

No pandas, criar novas colunas em um DataFrame é uma tarefa comum, essencial para enriquecer os dados com informações derivadas, categorizações ou transformações. Essa operação é direta, mas oferece muitas possibilidades.

Para se criar uma nova coluna em um DataFrame é necessário atribuirmos uma lista/Series de valores ou uma constante a uma nova chave do DataFrame. Da mesmo forma da atribuição, a quantidade de valores da lista precisa ser igual ao número de linhas totais do DataFrame.

### Criando colunas a partir de valores constantes

Para criar colunas a partir de um valor constante, ou seja, todas as linhas terão o mesmo valor para esta nova coluna podemos:

```python
df_data['Coluna Constante'] = 'Valor Constante'
df_data.head(3)
```

![Bloco 11](/assets/images/28-01-2025/Bloco 11.png)
_Retorno do `df_data` após a criação da nova coluna `Coluna Constante`_

Agora, a nova coluna `Coluna Constante` foi criada e ela contém em todas as 105018 linhas o mesmo valor, neste caso a string **'Constante'**.

### Criando colunas a partir de uma lista

Para criar colunas a partir de uma lista pré definida, basta atribuir ao DataFrame o novo campo com a lista.

```python
df_data['Coluna Lista'] = range(df_data.shape[0])
df_data.head(3)
```

![Bloco 12](/assets/images/28-01-2025/Bloco 12.png)
_Retorno do `df_data` após a criação da nova coluna `Coluna Lista`_

Agora, a nova coluna `Coluna Lista` foi criada e ela contém em todas as 105018, neste caso o intervalo entre número **0** até **105017**.

### Criando colunas a partir de uma lista menor

Se tentarmos atribuir uma lista menor do que a quantidade de linhas totais do DataFrame teremos um problema de Match. Exemplo, se tentarmos atribuir uma lista de 3 itens no DataFrame `df_data`:

```python
df_data['Lista Menor'] = [1,2,3]
```

![Bloco 13](/assets/images/28-01-2025/Bloco 13.png)
_Erro retornado após a tentativa de criação da nova coluna `Lista Menor`_

### Criando colunas a partir de outras colunas

Uma coluna pode ser criado com base em calculos matemátios e/ou condicionais dependentes de outros campos. Por exemplo, se tentarmos criar uma nova coluna chamada `Nova Nota Consumidor` onde o valor dela é a multiplicação do campo `Nota Consumidor` * **2**, fariamos o seguintes:

```python
df_data['Nova Nota do Consumidor'] = df_data['Nota do Consumidor'] * 2
df_data.head(3)
```

![Bloco 14](/assets/images/28-01-2025/Bloco 14.png)
_Retorno do `df_data` após a criação da nova coluna `Nova Nota do Consumidor`_

### Criando colunas condicionais

Uma coluna pode ser criado com base em condicionais dependentes de outros campos. Por exemplo, se tentarmos criar uma nova coluna chamada `Coluna Condicional` onde o valor dela é dependente do valor do campo `Nova Nota do Consumidor` sendo que, caso este seja maior ou igual a 5 retorna **'Boa'** caso contrário **Ruim**:

```python
df_data['Coluna Condicional'] = np.where(df_data['Nova Nota do Consumidor'] >= 5, 'Boa','Ruim')
df_data.head(3)
```

>Neste caso precisamos utilizar a biblioteca `numpy` na qual será tratada posteriormente
{: .prompt-tip }

![Bloco 15](/assets/images/28-01-2025/Bloco 15.png)
_Retorno do `df_data` após a criação da nova coluna `Coluna Condicional`_

### Dropando campos

Para fazer o drop de campos que não serão utilizados no DataFrame, basta utilizar o método `drop()`, da seguinte forma:

```python
df_data.drop(['Coluna Constante','Coluna Lista','Nova Nota do Consumidor'], axis=1, inplace=True) 
df_data.head(3)
## O atributo axis(0|1) serve para indicarmos se a exclusão sera coluna ou linha
## O Atributo inplace(True|False) serve para descartar a necessidade de criação de um novo DataFrame
```

![Bloco 16](/assets/images/28-01-2025/Bloco 16.png)
_Retorno do `df_data` após a remoção das novas colunas `Coluna Constante`, `Coluna Lista` e `Nova Nota do Consumidor`_

Quais as vantagens de se dropar campos de um DataFrame?
* **Flexibilidade** : Funciona tanto para remover linhas quanto colunas, com uma sintaxe consistente.
* **Controle sobre modificações** : Com `inplace=False` (padrão), mantém o DataFrame original intacto, retornando uma nova cópia com as alterações.
* **Limpeza de dados** : Facilita a remoção de dados indesejados ou irrelevantes, ideal para remoção de colunas redundantes ou outliers.
* **Evita problemas de inconsistência** : Ajuda a evitar falhas em pipelines de dados.

---

>Para download do notebook utilizado, acesse o [🔗Link](https://github.com/Lucas-SanBar/PyArq/blob/2aa31d0d6ee760a2b32280e11c1fc3f908c3b431/Desbravando%20Pandas/Parte%202%20-%20Atribuindo%20valores%20e%20criando%20atributos.ipynb)
{: .prompt-warning }
