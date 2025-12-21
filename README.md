# MVP - Sprint Engenharia de Dados
PUC-Rio | Pós-Graduação em Ciencia de Dados e Analytics

# 📊 UEFA Euro 2024 — Chuteiras & Gols


### 🎯 Objetivo do Trabalho

O objetivo deste projeto é construir um pipeline de dados em nuvem para analisar a relação entre **uso de chuteiras (adidas vs não adidas)** e **desempenho esportivo (gols)** na Eurocopa 2024.

A partir do dataset disponibilizado, busquei responder perguntas de negócio que conectam **marca esportiva, atletas, seleções e performance**, simulando um cenário real de análise em um **Data Warehouse moderno**.

Problema a ser resolvido

*Existe alguma predominância ou impacto observável do uso de chuteiras adidas em relação a quantidade de atletas e gols marcados na Eurocopa 2024?*

## ❓ Perguntas a serem respondidas

1. Quantos atletas usam chuteira adidas?
2. Quantos gols foram feitos com chuteiras adidas?
3. Qual seleção tem mais jogadores que NÃO usam adidas?
4. Quantos gols foram feitos com chuteiras que não são adidas?
5. Qual o total de modelos de chuteira utilizados no campeonato?

## ☁️ Plataforma Utilizada

- Databricks: Free Edition
- Linguagem: Python (PySpark + Pandas quando necessário)
- Armazenamento: Delta Lake
- Arquitetura em camadas: Bronze / Silver / Gold

## 🔍 1. Busca pelos Dados

Os dados foram obtidos a partir de um repositório público no GitHub, disponibilizado em formato CSV.

📌 Fonte original:

- Dataset: UEFA Euro 2024
- Método de acesso: HTTP (raw GitHub)

Essa etapa garante:

- Rastreabilidade
- Reprodutibilidade
- Transparência da origem dos dados

## 📥 2. Coleta dos Dados

Na coleta, o arquivo CSV é ingerido diretamente no Databricks utilizando Spark.

**Estratégia adotada**
- Leitura do CSV via spark.read
- Definição explícita de schema (quando necessário)
- Salvamento inicial como Delta Table (Bronze)

## 📦 Camada Bronze

- Dados brutos
- Sem transformações
- Apenas padronização mínima (encoding, separador, headers)

## 🧱 3. Modelagem dos Dados

**⭐ Modelo Dimensional - Esquema Estrela**

O modelo foi desenhado no formato Star Schema, visando:
- Melhor performance analítica
- Facilidade de leitura
- Compatibilidade com BI e SQL analítico

### 📌 Tabela Fato

Fato_Gols

- id_atleta
- id_partida
- id_chuteira
- quantidade_gols

### 📐 Tabelas Dimensão

**Dim_Atleta**
- id_atleta
- nome_atleta
- seleção

**Dim_Chuteira**
- id_chuteira
- marca
- modelo
  
**Dim_Partida**
- id_partida
- data
- fase
- seleção_a
- seleção_b

## 📚 Catálogo de Dados

**Dim_Atleta**
*Atributo*
- nome_atleta
- seleção

*Tipo*
- String

*Domínio*
- Texto
- País participante

**Dim_Chuteira**
*Atributo*
- marca
- modelo
- marca_flag

*Tipo*
- String
- Boolean

*Domínio*
- adidas
- nike
- puma
- etc
- Texto
- não adidas

**Fato_Gols**
 *Atributo*
- quantidade_gols

*Tipo*
- Integer

*Domínio*
- Min: 0
- - Max esperado: 5

## 🔗 Linhagem dos Dados

- Origem: CSV público no GitHub
- Técnica: ingestão direta + normalização
- Transformações: limpeza, deduplicação, criação de chaves substitutas

## 🚚 4. Carga (ETL)

## 🔄 Pipeline ETL
**Extração**
- Leitura do CSV bruto

**Transformação (Camada Silver)**
- Padronização de marcas (upper, trim)
- Criação de flag: is_adidas
- Remoção de duplicatas
- Normalização em dimensões
- Criação de IDs surrogate

**Carga (Camada Gold)**
- Tabelas finais em Delta
- Modelo estrela pronto para análise

**📌 Aqui rola join, group by, agregações e conciliações entre atletas, gols e chuteiras.**

## 📈 5. Análise

**a. Qualidade dos Dados**
*Problemas identificados*
- Nomes de marcas inconsistentes (Adidas, adidas, ADIDAS)
- Possíveis valores nulos em modelo de chuteira
- Atletas sem gols (valor 0 — ok, mas precisa atenção)

*Tratativas*
- Normalização de texto
- Preenchimento de valores nulos como "Modelo não informado"
- Validação de tipos numéricos

**Conclusão**
✅ Após tratamento, os dados estão aptos para análise sem viés significativo.

**b. Solução do Problema**
📌 1. Quantos atletas usam chuteira adidas?
- Contagem distinta de atletas onde marca = 'adidas'

💬 Discussão:
Mostra a presença de mercado da adidas entre atletas da Euro.

📌 2. Quantos gols foram feitos com chuteiras adidas?
- Soma de gols associados à marca adidas

💬 Discussão:
Ajuda a avaliar representatividade esportiva, não causalidade.

📌 3. Qual seleção tem mais jogadores sem adidas?
- Agrupamento por seleção

Filtro marca != 'adidas'

💬 Discussão:
Indica diversidade de patrocínios por país.

📌 4. Quantos gols foram feitos com chuteiras não adidas?
- Soma de gols onde marca != 'adidas'

💬 Discussão:
Serve como contraponto direto à análise da adidas.

📌 5. Total de modelos de chuteira
- count(distinct modelo)

💬 Discussão:
Mostra o nível de diversidade tecnológica no torneio.

## 🧠 Discussão Final
A análise mostra que:
- A adidas possui forte presença entre atletas
- Os gols estão distribuídos entre marcas, sem evidência de superioridade técnica
- Algumas seleções apresentam maior diversidade de patrocinadores
- O modelo estrela facilitou MUITO a análise

## 📌 Conclusão geral:
O pipeline construído permitiu transformar dados brutos em insights claros, com qualidade, rastreabilidade e escalabilidade.
