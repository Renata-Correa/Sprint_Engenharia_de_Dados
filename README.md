# MVP - Sprint Engenharia de Dados
PUC-Rio | Pós-Graduação em Ciencia de Dados e Analytics

*Projeto desenvolvido para o Curso de Pós-Graduação em Ciência de Dados e Analytics – Sprint Engenharia de Dados da Pontifícia Universidade Católica do Rio de Janeiro (PUC/RJ), como requisito parcial para obtenção do título em Latu Sensu, sob a orientação dos Profs.: Victor Almeida e José Boaro.*


# 📊 UEFA Euro 2024 — Pipeline de Engenharia de Dados


### 📌 Visão Geral

Este projeto implementa um pipeline completo de Engenharia de Dados utilizando Databricks, a partir de um dataset público da UEFA Euro 2024 hospedado no GitHub.

O pipeline segue a arquitetura Medallion (Bronze, Silver, Gold), garantindo ingestão segura, dados confiáveis e métricas prontas para análise.


## 🧱 Arquitetura do Pipeline

GitHub (CSV)
   ↓
Unity Catalog Volume
   ↓
Bronze Layer (Delta)
   ↓
Silver Layer (Delta)
   ↓
Gold Layer (Delta)


## 🛠️ Tecnologias Utilizadas

- Databricks
- Python
- GitHub


## 📥 Fonte de Dados

Dataset: UEFA Euro 2024

Origem: GitHub (arquivo CSV)


## 🧪 Camadas do Pipeline
🥉 Bronze - Raw Data

- Ingestão do CSV a partir do GitHub

- Armazenamento inicial no Unity Catalog Volume

- Persistência em tabela Delta sem transformações

Tabela:

bronze_uefa_euro_2024


## 🥈 Silver — Dados Tratados

- Remoção de duplicados

- Tratamento de valores nulos

- Padronização de tipos de dados

- Dados prontos para análise exploratória

Tabela:

silver_uefa_euro_2024


## 🥇 Gold — Métricas de Negócio

- Agregações por seleção

- Criação de métricas analíticas (gols, chutes, posse média, partidas)

Tabela:

gold_uefa_team_metrics


## 📊 Análise Exploratória

A EDA foi realizada sobre a camada Silver, explorando:

- Distribuição de gols por seleção

- Eficiência ofensiva (gols / chutes)

- Relação entre posse de bola e desempenho

- As visualizações foram feitas utilizando o recurso nativo do Databricks (display).

## ⏰ Automação

O pipeline pode ser orquestrado via Databricks, com execução sequencial:

- Ingestão Bronze

- Transformação Silver

- Criação da Gold

## 🚀 Próximos Passos

- Validações de qualidade de dados

- Versionamento de schemas

- Monitoramento de jobs

- Integração com ferramentas de BI

## 📌 Conclusão

Este projeto demonstra um pipeline moderno de engenharia de dados, alinhado às melhores práticas de mercado e pronto para escalar em ambientes corporativos.
