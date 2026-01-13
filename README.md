# ⚽ Engenharia de Dados & Analytics — UEFA Euro 2024

# MVP - Sprint Engenharia de Dados
PUC-Rio | Pós-Graduação em Ciencia de Dados e Analytics
## 📝 Nota final: 9.5

## 👩‍💻 Autora

*Rê Corrêa* | Gerente de Projetos na act.3 | Engenharia de Dados | Analytics

Apaixonada por dados, pipelines bem feitos e dashboards que contam histórias 📊✨

### **🎯 Objetivo do Projeto**

O objetivo deste trabalho é analisar a relação entre o **uso de marcas de chuteiras e o desempenho esportivo dos atletas na Eurocopa 2024**, utilizando dados estruturados armazenados em um ambiente de Data Warehouse.

A partir da base `workspace_renatacorrea.data.uefa_euro_2024` (também disponível em [UEFA Euro 2024.csv ](https://raw.githubusercontent.com/Renata-Correa/Sprint_Engenharia_de_Dados/refs/heads/main/UEFA%20Euro%202024.csv)), busquei responder perguntas de negócio relacionadas a exposição de marcas esportivas, distribuição entre seleções e impacto direto na quantidade de gols marcados.

**Problema a ser resolvido**

*Existe alguma predominância ou impacto observável do uso de chuteiras adidas em relação a quantidade de atletas e gols marcados na Eurocopa 2024?*

## ❓ Perguntas a serem respondidas:

1. Quantos atletas usam chuteira adidas?
2. Quantos gols foram feitos com chuteiras adidas?
3. Qual seleção possui mais jogadores usando chuteiras que NÃO são adidas?
4. Quantos gols foram feitos com chuteiras que não são adidas?
5. Qual o total de modelos diferentes de chuteira utilizados no campeonato?

### 🧱 Plataforma Utilizada

- **☁️ Ambiente de Processamento**: Databricks Free Edition.
- **🛠️ Linguagens:** SQL e Python.
- **📚 Camada Analítica**: Apache Spark.
- **📂 Armazenamento:** Data Lake / Data Warehouse.
- **🔍 Fonte de Dados:** Base interna corporativa (act.3 Brasil – todos os direitos reservados).

### 🗂️ 1. Busca pelos Dados

Os dados utilizados neste projeto são provenientes de uma base de dados interna, mantida pela empresa act.3 Brasil, contendo informações consolidadas sobre:
- Atletas participantes da Eurocopa 2024.
- Seleções.
- Marcas e modelos de chuteiras.
- Gols marcados durante a competição.

A base já se encontra armazenada no ambiente analítico, acessível via tabela:
`workspace_renatacorrea.data.uefa_euro_2024`

Você também pode encontrar o arquivo em [UEFA Euro 2024.csv](https://raw.githubusercontent.com/Renata-Correa/Sprint_Engenharia_de_Dados/refs/heads/main/UEFA%20Euro%202024.csv).

### 📥 2. Coleta

A coleta foi realizada por meio de ingestão controlada em ambiente corporativo, garantindo:
- Integridade dos dados.
- Padronização de estrutura.
- Governança e controle de acesso.

No Databricks, a coleta ocorre via leitura direta da tabela:
`SELECT *
FROM workspace_renatacorrea.data.uefa_euro_2024;`

`sql
SELECT *
FROM workspace_renatacorrea.data.uefa_euro_2024;`

### ⭐ 3. Modelagem de Dados - Esquema Estrela

### 🎯 Tabela Fato - F_Goals

Representa os eventos de gols marcados.

| Campo     | Tipo | Descrição               |
| --------- | ---- | ----------------------- |
| goal_id   | INT  | Identificador do evento |
| player_id | INT  | FK do jogador           |
| team_id   | INT  | FK da seleção           |
| boot_id   | INT  | FK da chuteira          |
| goals     | INT  | Quantidade de gols      |

### 📐 Tabelas Dimensão

**D_Player**

| Campo       | Tipo   | Domínio          |
| ----------- | ------ | ---------------- |
| player_id   | INT    | Chave substituta |
| player_name | STRING | Nome do atleta   |

**D_Team**

| Campo     | Tipo   | Domínio          |
| --------- | ------ | ---------------- |
| team_id   | INT    | Chave substituta |
| team_name | STRING | Nome da seleção  |

**D_Boot**

| Campo      | Tipo   | Domínio                  |
| ---------- | ------ | ------------------------ |
| boot_id    | INT    | Chave substituta         |
| boot_brand | STRING | adidas, nike, puma, etc. |
| boot_model | STRING | Texto livre              |

### 📖 Catálogo de Dados

| Atributo  | Tipo   | Domínio Esperado           |
| --------- | ------ | -------------------------- |
| Player    | STRING | Não nulo                   |
| Team      | STRING | Lista de seleções          |
| BootBrand | STRING | adidas, nike, puma, outras |
| BootModel | STRING | Modelos comerciais         |
| Goals     | INT    | 0 ≤ valor ≤ 5              |

### 📊 Dados Numéricos

**Goals:**
- mínimo esperado = 0
- máximo esperado = 5

### 🔗 Linhagem dos Dados

- **Origem:** Base interna act.3 Brasil.
- **Técnica de composição:** Consolidação de dados esportivos oficiais mais o enriquecimento com informações de equipamentos esportivos.
- **Destino**: Data Warehouse corporativo acessado via Databricks.

### 🔄 4. Carga — Pipeline ETL

**🔹 Extração**
- Leitura direta da tabela bruta no ambiente corporativo.

**🔹 Transformação**

Principais transformações aplicadas:
- Padronização de texto (BootBrand).
- Tratamento de valores nulos em Goals.
- Remoção de duplicidades.
- Normalização para modelo dimensional.

`df = spark.sql(
    "SELECT * FROM workspace_renatacorrea.data.uefa_euro_2024"
)
display(df)`

`from pyspark.sql.functions import initcap, trim, col`

`df = df.withColumn(
    "BootBrand",
    initcap(trim(col("BootBrand")))
).fillna(
    {"Goals": 0}
)`

**🔹 Carga**

Os dados transformados são carregados em tabelas analíticas otimizadas para consulta e visualização.

### 📊 5. Análise de Dados

**🧪 Qualidade de Dados**

| Atributo  | Problema                | Solução               |
| --------- | ----------------------- | --------------------- |
| BootBrand | Inconsistência de texto | Normalização          |
| Goals     | Valores nulos           | Substituição por zero |
| Player    | Possível duplicidade    | Contagem distinta     |

✔️ Após tratamento, os dados tornam-se confiáveis para análise.

### 🧠 Solução do Problema

[MVP_Engenharia_de_Dados_Documentação/prints/Dash interativo.png](https://github.com/Renata-Correa/Sprint_Engenharia_de_Dados/blob/main/MVP_Engenharia_de_Dados_Documenta%C3%A7%C3%A3o/prints/Dash%20interativo.png)

🥾 Atletas usando chuteira adidas

`df = spark.sql(
    """
    SELECT COUNT(DISTINCT Player)
    FROM workspace_renatacorrea.data.uefa_euro_2024
    WHERE LOWER(`Boot Brand`) = 'adidas'
    """
)
display(df)`

⚽ Gols com chuteiras adidas

`df = spark.sql(
    """
    SELECT SUM(try_cast(Goals AS INT)) AS total_goals
    FROM workspace_renatacorrea.data.uefa_euro_2024
    WHERE LOWER(`Boot Brand`) = 'adidas'
    """
)
display(df)`

🏴 Seleção com mais jogadores sem adidas

`%sql
SELECT 'Team', COUNT(DISTINCT Player) AS qtd
FROM workspace_renatacorrea.data.uefa_euro_2024
WHERE LOWER("Boot Brand") <> 'adidas'
GROUP BY 'Team'
ORDER BY qtd DESC;`

⚽ Gols sem adidas

`%sql
SELECT SUM(try_cast(Goals AS DOUBLE))
FROM workspace_renatacorrea.data.uefa_euro_2024
WHERE LOWER(`Boot Brand`) <> 'adidas';`

👟 Total de modelos

`%sql
SELECT COUNT(DISTINCT `Boot Type`) AS total_modelos
FROM workspace_renatacorrea.data.uefa_euro_2024;`

### 🚀 Como Executar o Projeto

1. Crie uma conta no Databricks Free Edition.
2. Faça upload do notebook .ipynb.
3. Carregue o arquivo CSV na área de dados.
4. Execute as células na ordem.
5. Explore os dados e os insights.

### 📈 Visualização (Dashboards)

Recomenda-se o uso de:
- Gráfico de barras: distribuição de marcas.
- Tabela ranqueada: seleções por marca.
- KPI cards: gols adidas vs não adidas.
Essas visualizações podem ser criadas diretamente no Databricks SQL Dashboard.

## 🧠 Análise Final
A análise mostra que:
- A adidas domina o uso de chuteiras entre os atletas.
- Os gols estão distribuídos entre marcas, sem evidência de superioridade técnica.
- Algumas seleções apresentam maior diversidade de patrocinadores.
- Grande variedade de modelos utilizados no torneio.
- O modelo estrela facilitou MUITO a análise.

## 📈 Conclusão geral:
O pipeline construído permitiu transformar dados brutos em insights claros, com qualidade, rastreabilidade e escalabilidade.

A análise demonstra que, embora a adidas possua forte presença entre os atletas, outras marcas também exercem impacto significativo no desempenho das seleções, tanto em número de jogadores quanto em gols marcados.

**O pipeline construído garante:**
- Escalabilidade
- Governança
- Reprodutibilidade
- Clareza analítica

📸 Os prints do dashboard estão disponíveis na pasta [MVP_Engenharia_de_Dados_Documentação/prints](https://github.com/Renata-Correa/Sprint_Engenharia_de_Dados/tree/main/MVP_Engenharia_de_Dados_Documenta%C3%A7%C3%A3o/prints).
