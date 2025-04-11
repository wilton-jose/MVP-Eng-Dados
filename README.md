# **MVP Engenharia de Dados - Análise do Consumo de Energia no Brasil (2018-2020)**  

**Autor:** [Wilton José da Silva Júnior](https://github.com/wilton-jose)  
**Instituição:** PUC-RIO  

## **📌 Visão Geral**  

Este projeto tem como objetivo analisar o consumo de energia elétrica no Brasil entre 2018 e 2020, utilizando dados públicos da **CCEE (Câmara de Comercialização de Energia Elétrica)**. O pipeline de dados foi construído em um ambiente **Databricks**, seguindo a arquitetura **Lakehouse (Bronze → Silver → Gold)** e implementando um modelo **Star Schema** para análise dimensional.  

🔹 **Principais perguntas respondidas:**  
✔ Como o consumo varia por região/estado ao longo do ano?  
✔ Quais ramos de atividade têm os maiores consumos?  
✔ Existem padrões sazonais significativos no consumo?  
✔ Quais estados apresentam maior crescimento no consumo?  

---

## **📂 Estrutura do Projeto**  

### **🎯 Objetivos**  
- Construir um pipeline de dados escalável para ingestão, transformação e análise.  
- Identificar tendências e padrões no consumo de energia por estado, ramo de atividade e período.  
- Gerar insights para otimização da distribuição energética.  

### **📊 Arquitetura do Pipeline**  

| **Camada**  | **Descrição** | **Tecnologias** |  
|-------------|--------------|----------------|  
| **Bronze**  | Dados brutos ingeridos da fonte (CSV) | Spark, Delta Lake |  
| **Silver**  | Dados limpos, modelados em esquema estrela | Spark SQL, Delta Lake |  
| **Gold**    | Agregações e análises prontas para visualização | Spark SQL, Delta Lake |  

### **📋 Modelagem de Dados**  

#### **Bronze (`bronze_cmee_br`)**  
- Armazena os dados brutos da CCEE.  
- Inclui metadados como `ingestion_date` e `file_name`.  

#### **Silver (Esquema Estrela)**  
- **Tabelas de dimensão:** `dim_data`, `dim_classe`, `dim_ramo`, `dim_submercado`, `dim_estado`  
- **Tabela fato:** `fato_consumo`  

#### **Gold (Análises Prontas)**  
- `gold_consumo_por_estado`  
- `gold_tendencia_mensal`  
- `gold_top_ramos`  
- `gold_tendencia_mensal_avancada`  

---

## **⚙️ Tecnologias Utilizadas**  

- **Apache Spark** (ETL e processamento distribuído)  
- **Delta Lake** (Armazenamento em Lakehouse)  
- **Databricks** (Plataforma de execução)  
- **Python** (Análise e visualização)  
- **Matplotlib/Seaborn** (Gráficos)  

---

## **📈 Principais Insights**  

### **1️⃣ Consumo por Estado**  
- **São Paulo (SP)** lidera em consumo total (**9,6M MWm**).  
- **Acre (AC)** tem o menor consumo (**72K MWm**).  

### **2️⃣ Tendência Mensal**  
- **Pico em janeiro** (alta demanda no verão).  
- **Queda em junho** (clima mais ameno).  

### **3️⃣ Ramos com Maior Consumo**  
1. **ACR (Ambiente de Contratação Regulada)** → **25,2M MWm**  
2. **Metalurgia** → **2,7M MWm**  
3. **Químicos** → **1,1M MWm**  

---

## **🚀 Como Executar o Projeto**  

1. **Configurar o ambiente Databricks:**  
   - Criar um cluster com **Apache Spark 3.x**.  
   - Configurar acesso ao **DBFS (Databricks File System)**.  

2. **Importar os notebooks:**  
   - Disponíveis em: [MVP-Eng-Dados](https://github.com/wilton-jose/MVP-Eng-Dados)  

3. **Executar o pipeline:**  
   - Rodar os notebooks na ordem:  
     - `1_Ingestao_Bronze`  
     - `2_Processamento_Silver`  
     - `3_Analise_Gold`  

4. **Visualizar resultados:**  
   - Dashboards no Databricks ou exportar para Power BI/Tableau.  

---

## **📌 Próximos Passos**  

✅ **Adicionar dados meteorológicos** para correlacionar clima e consumo.  
✅ **Criar um dashboard interativo** com filtros por período e região.  
✅ **Automatizar o pipeline** com agendamento (Airflow/Databricks Jobs).  

---

## **📜 Licença**  
Este projeto utiliza dados públicos da **CCEE**. Consulte as políticas de uso em: [CCEE Dados Abertos](https://www.ccee.org.br/dados-abertos)  

---

**🔗 Repositório:** [GitHub - MVP Engenharia de Dados](https://github.com/wilton-jose/MVP-Eng-Dados)  
