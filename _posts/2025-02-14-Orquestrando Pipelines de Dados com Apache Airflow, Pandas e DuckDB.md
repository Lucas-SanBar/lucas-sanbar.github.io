---
title: "Orquestrando Pipelines de Dados com Apache Airflow, Pandas e DuckDB"
description: Aprenda a criar um pipeline de dados orquestrado com Apache Airflow, utilizando PostgreSQL como origem, Pandas para transformação e DuckDB como destino.
date: 2025-02-11
categories: [Pipeline de Dados, Apache Airflow]
tags: [airflow, duckdb, postgresql, python, pandas, dag]
image: 
  path: /assets/images/20-02-2025/Capa-20-02-2025.png
---

# Introdução

Com o crescimento exponencial dos dados, a necessidade de processá-los de forma eficiente nunca foi tão importante. Empresas e profissionais de dados enfrentam desafios constantes para extrair informações a partir de grandes volumes de dados armazenados em bancos relacionais, planilhas e outros formatos.

Neste artigo, vamos explorar a construção de um pipeline de dados, combinando algumas das melhores ferramentas disponíveis no mercado:
1. **PostgreSQL** como banco de origem, onde os dados brutos serão armazenados;
2. **Pandas** para processar e transformar os dados;
3. **DuckDB** como banco de destino, ideal para consultas analíticas eficientes;
4. **Airflow** para orquestrar e automatizar a execução do pipeline;

Vamos entender como essas ferramentas se complementam para criar um fluxo de dados robusto. O pipeline que iremos construir segue as três etapas clássicas de um **ETL** (Extract, Transform, Load):

1. **Extração:** Coletamos os dados diretamente do **PostgreSQL** utilizando Python.
2. **Transformação:** Processamos e limpamos os dados com **Pandas**, aplicando regras de negócio e agregações conforme necessário.
3. **Carregamento:** Salvamos os dados pré transformados e transformados no **DuckDB**, aproveitando sua performance otimizada para análises.

Para garantir que tudo funcione de forma automática e escalável, utilizaremos o Apache **Airflow** para orquestrar o pipeline, agendar execuções e monitorar possíveis falhas.

Ao final deste artigo, você terá um pipeline funcional que pode ser adaptado para diversos cenários, seja para processar dados de vendas, logs de sistemas ou qualquer outro conjunto de informações estruturadas.

# O que é o DuckDB?

## Como instalar?

--Vantagens de se utilizar o DuckDB?

# O que é o Apache Airflow?

## Como instalar?

--Vantagens de se utilizar o Apache Airflow?

# 1. Definição do objetivo

Antes de começar, defina claramente:

* O que você quer processar? Exemplo: transformar dados brutos de vendas do PostgreSQL em um formato otimizado no DuckDB.
* Com que frequência o pipeline será executado? Exemplo: diariamente ou em tempo real?
* Quais transformações são necessárias? Exemplo: limpeza de dados, agregações, cálculo de métricas.

# 2. Extrutura do Pipeline

A pipeline será composta por três etapas principais:

1. Extração (PostgreSQL → Pandas)
* Utilize a biblioteca psycopg2 ou sqlalchemy para se conectar ao PostgreSQL.
* Leia os dados usando pandas.read_sql_query().
2. Transformação (Pandas)
* Limpeza e tratamento de dados (exemplo: remoção de valores nulos, renomeação de colunas).
* Transformações e cálculos específicos para seu caso de uso.
3. Carregamento (Pandas → DuckDB)
* Utilize duckdb para armazenar os dados transformados.
* Grave os dados usando duckdb.write_parquet() ou diretamente para uma tabela DuckDB.

# 3. Orquestração com Apache Airflow

* Criar um DAG no Airflow com três tarefas:
  1. Extract: Pega os dados do PostgreSQL.
  2. Transform: Processa os dados com Pandas.
  3. Load: Salva os dados no DuckDB.
* Configurar dependências entre as tarefas e definir a periodicidade da execução.

# 4. Testes e Monitoramento
* Valide se os dados estão sendo transformados corretamente.
* Configure alertas no Airflow para falhas no pipeline.

