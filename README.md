# MVP-Engenharia-de-Dados

Repositório desenvolvido para a execução do **MVP da Sprint de Engenharia de Dados**
do curso de **Ciências de Dados e Analytics**, com foco na construção de um pipeline
de dados em nuvem utilizando a plataforma Databricks.

---

## MVP – Pipeline de Dados em Nuvem com Databricks
### Breast Cancer Wisconsin (Diagnostic)


Este projeto tem como objetivo o desenvolvimento de um **MVP de pipeline de dados em nuvem**, utilizando a plataforma **Databricks**, aplicando boas práticas de **Engenharia de Dados** e o **modelo medalhão (Bronze, Silver e Gold)**.

O pipeline contempla todas as etapas exigidas: **coleta, armazenamento, modelagem, carga, análise de qualidade e análise exploratória**, com versionamento do código via **GitHub**.

---

## 🎯 Objetivo do Trabalho

O objetivo deste MVP é **construir um pipeline de dados confiável e organizado**, capaz de suportar análises sobre características celulares associadas ao diagnóstico de câncer de mama, demonstrando domínio dos conceitos de:

- Pipelines de ETL em nuvem  
- Arquitetura em camadas (modelo medalhão)  
- Qualidade de dados  
- Modelagem e documentação  
- Análise baseada em dados armazenados  

---

## ❓ Perguntas que o MVP Busca Responder

As análises realizadas neste projeto buscam responder às seguintes perguntas:

1. Qual é a distribuição de casos benignos e malignos no conjunto de dados?
2. Existem diferenças relevantes nas características celulares entre tumores benignos e malignos?
3. O conjunto de dados apresenta problemas de qualidade, como valores nulos ou registros duplicados?
4. Os valores das variáveis numéricas estão dentro de domínios esperados (sem valores incoerentes)?
5. O dataset apresenta consistência suficiente para suportar análises confiáveis?

> **Observação:** Nem todas as perguntas precisam ser respondidas com o mesmo nível de profundidade. As que não forem plenamente respondidas são discutidas na autoavaliação.

---

## 🗂️ Conjunto de Dados Utilizado

- **Nome:** Breast Cancer Wisconsin (Diagnostic)  
- **Origem:** Biblioteca `scikit-learn`  
- **Descrição:**  
  Dataset acadêmico contendo medidas numéricas extraídas de imagens de biópsias de câncer de mama. Cada registro representa uma observação e a variável alvo indica se o tumor é benigno ou maligno.  
- **Licença:** Dataset público, amplamente utilizado para fins educacionais e acadêmicos.

---

## ☁️ Plataforma e Tecnologias Utilizadas

- **Plataforma de Dados:** Databricks (Free Edition)  
- **Armazenamento:** Delta Lake  
- **Linguagens:**  
  - PySpark (ETL e transformações)  
  - SQL (consultas analíticas)  
- **Versionamento:** GitHub (integração via Git Folder no Databricks)

---

## 🏗️ Arquitetura do Pipeline (Modelo Medalhão)

O pipeline foi estruturado conforme o **modelo medalhão**, separando responsabilidades e aumentando a confiabilidade dos dados:


### 🔹 Staging
- Utilizado para inspeção inicial dos dados em memória.  
- Validação do schema e amostras.  
- Não há persistência em Delta nesta etapa.

### 🔹 Bronze
- Persistência inicial em Delta Lake.  
- Padronização técnica dos nomes das colunas.  
- Inclusão de metadados de ingestão (`ingestion_timestamp`, `ingestion_date`).  
- Nenhuma transformação semântica aplicada.

### 🔹 Silver
- Camada responsável por **qualidade de dados**.  
- Enriquecimento semântico (`diagnosis_desc`).  
- Verificações de:
  - valores nulos  
  - duplicidades  
  - domínio da variável alvo  
  - sanidade dos valores numéricos  
- Dados considerados confiáveis para análise.

### 🔹 Gold
- Camada de **consumo analítico**.  
- Cópia direta da Silver, sem novas transformações.  
- Mantém aderência ao modelo medalhão.  
- Base estável para análises e documentação.  
- Contém o **Catálogo de Dados** e a **Linhagem**.

---

## 📊 Qualidade dos Dados

A análise de qualidade realizada na camada Silver permitiu concluir que:

- **Completude:** não há valores nulos no dataset.  
- **Unicidade:** não foram identificados registros duplicados.  
- **Consistência:** a variável `diagnosis` contém apenas os valores esperados (0 e 1).  
- **Sanidade:** não há valores negativos ou incoerentes nas variáveis numéricas.

Esses resultados indicam que o dataset apresenta **qualidade adequada para uso analítico**, não sendo necessária a aplicação de tratamentos corretivos adicionais.

---

## 📘 Catálogo de Dados

O Catálogo de Dados foi documentado na camada **Gold**, pois esta representa a camada de consumo do pipeline, com dados já tratados, estáveis e semanticamente claros.

O catálogo descreve:
- nome e tipo das colunas  
- significado de cada atributo  
- domínios esperados  
- metadados técnicos de ingestão  

---

## 🔍 Análise dos Dados

As análises foram realizadas em um notebook separado, consumindo **exclusivamente a camada Gold**, conforme boas práticas do modelo medalhão.

As consultas realizadas permitiram:
- analisar a distribuição dos diagnósticos  
- comparar estatísticas entre tumores benignos e malignos  
- responder às perguntas definidas no objetivo do trabalho  

As evidências das análises são apresentadas por meio de consultas SQL e visualizações no Databricks.

---


## 📂 Estrutura do Repositório

Abaixo está apresentada a estrutura do repositório, organizada por notebooks,
cada um representando uma etapa do pipeline de dados conforme o modelo medalhão:

```text
MVP-Engenharia-de-Dados/
│
├── README.md
│
├── 01_preparacao_ambiente
│   └── Configuração inicial do ambiente e criação dos schemas
│
├── 02_download_dados
│   └── Coleta e carregamento do dataset Breast Cancer Wisconsin
│
├── 03_bronze_breast_cancer
│   └── Padronização técnica e persistência inicial dos dados (camada Bronze)
│
├── 04_silver_breast_cancer
│   └── Tratamento, validação de qualidade e enriquecimento semântico (camada Silver)
│
├── 05_gold_breast_cancer
│   └── Camada de consumo analítico e documentação do catálogo de dados (camada Gold)
│
└── 06_analise
    └── Análises exploratórias e respostas às perguntas do objetivo 
```

Todos os notebooks foram versionados diretamente a partir do Databricks via integração com o GitHub.

---

## 📝 Autoavaliação (Resumo)

O MVP atendeu aos objetivos propostos, permitindo a construção de um pipeline completo em nuvem, com organização em camadas, qualidade de dados validada e análises consistentes.

As principais dificuldades estiveram relacionadas ao entendimento inicial da plataforma Databricks e das restrições do Delta Lake, superadas ao longo do desenvolvimento.

Como trabalhos futuros, o pipeline poderia ser expandido com:
- ingestão de dados externos via arquivos ou APIs  
- automação de cargas recorrentes  
- materialização de métricas analíticas na camada Gold  
- integração com ferramentas de visualização  

---

## ✅ Conclusão

Este projeto demonstrou a aplicação prática de conceitos de **Engenharia de Dados em nuvem**, utilizando Databricks e o modelo medalhão para garantir organização, qualidade e confiabilidade dos dados, atendendo plenamente aos requisitos propostos para o MVP.

