# Documentação do Projeto MVP Engenharia de Dados

## 1. Visão Geral do Projeto

### Objetivo
Este projeto tem como objetivo principal otimizar a alocação de recursos energéticos no setor elétrico brasileiro, através da análise de padrões de consumo de energia elétrica por região, classe de consumidor e ramo de atividade.

### Contexto Setorial
O setor energético brasileiro enfrenta desafios significativos na distribuição e transmissão de energia, com perdas que impactam a eficiência do sistema. A otimização da alocação de recursos energéticos é crucial para reduzir custos e melhorar a confiabilidade do fornecimento.

### Perguntas de Negócio
- Como o consumo varia por região/estado ao longo do ano?
- Quais ramos de atividade têm os maiores consumos?
- Quais estados apresentam maior crescimento no consumo?

### Arquitetura do Pipeline
O pipeline de dados é estruturado em três camadas:
- **Bronze**: Dados brutos ingeridos da fonte
- **Silver**: Dados tratados e modelados em formato dimensional
- **Gold**: Dados agregados para análise e visualização

Tecnologias utilizadas:
- Apache Spark para processamento distribuído
- Delta Lake para armazenamento de dados
- Python para desenvolvimento das transformações

### Fontes de Dados
- **Fonte**: CCEE (Câmara de Comercialização de Energia Elétrica)
- **Período**: 2010-2023
- **Formato original**: CSV
- **Frequência de atualização**: Mensal
- **Licença**: Dados abertos
- **Método de obtenção**: Download direto do portal da CCEE

## 2. Descrição das Tabelas

### Camada Bronze

#### Tabela `bronze_cmee_br`
- **Localização**: `/mnt/data/bronze/cmee_br`
- **Descrição**: Armazena os dados brutos de consumo de energia elétrica por classe, ramo de atividade, submercado e estado.

| Coluna | Tipo | Formato | Restrições | Descrição |
|--------|------|---------|------------|-----------|
| Data | DATE | YYYY-MM-DD | NOT NULL | Data da medição |
| Classe | STRING | - | NOT NULL | Tipo de consumidor (Residencial, Industrial, etc.) |
| Ramo_de_atividade | STRING | - | - | Ramo de atividade do consumidor |
| Submercado | STRING | - | NOT NULL | Região geográfica |
| Estado | STRING | - | NOT NULL | Sigla do estado |
| Consumo_MWm | DOUBLE | - | NOT NULL, >= 0 | Consumo em megawatt-hora |
| ingestion_date | TIMESTAMP | - | NOT NULL | Data/hora da ingestão |
| file_name | STRING | - | NOT NULL | Nome do arquivo de origem |

**Exemplo de dados**:
```
Data,Classe,Ramo_de_atividade,Submercado,Estado,Consumo_MWm
2010-01-01,Residencial,null,Sudeste/Centro-Oeste,SP,1000.0
2010-01-01,Industrial,Indústria Química,Sudeste/Centro-Oeste,RJ,5000.0
```

### Camada Silver

#### Tabelas de Dimensão
- `silver_dim_data`: Dimensão temporal
- `silver_dim_classe`: Classes de consumidores
- `silver_dim_ramo`: Ramos de atividade
- `silver_dim_submercado`: Submercados energéticos
- `silver_dim_estado`: Estados brasileiros

#### Tabela de Fato `silver_fato_consumo`
- **Localização**: `/mnt/silver/cmee/fato_consumo`
- **Descrição**: Relaciona os dados de consumo com as dimensões através de chaves estrangeiras.

**Exemplo de dados**:
```
data_completa,id_classe,id_ramo,id_submercado,id_estado,consumo_mwm,ingestion_date
2010-01-01,1,null,1,1,1000.0,2023-11-27T12:00:00.000Z
2010-01-01,2,1,1,2,5000.0,2023-11-27T12:00:00.000Z
```

### Camada Gold
- `gold_consumo_por_estado`: Consumo agregado por estado
- `gold_tendencia_mensal`: Tendências mensais de consumo
- `gold_top_ramos`: Ramos de atividade com maior consumo
- `gold_tendencia_mensal_avancada`: Análise avançada de tendências

## 3. Detalhes do Pipeline

### Ingestão de Dados
1. Download dos dados da CCEE
2. Configuração da sessão Spark
3. Carregamento dos dados na tabela `bronze_cmee_br`

### Transformações na Camada Silver
1. Criação das tabelas de dimensão
2. Construção da tabela de fato
3. Limpeza e tratamento de dados
4. Junções entre tabelas
5. Agregações e cálculos

### Criação da Camada Gold
- `calcular_consumo_por_estado`: Agrega consumo por estado
- `calcular_tendencia_mensal`: Calcula tendências temporais
- `calcular_top_ramos`: Identifica os ramos com maior consumo
- `criar_gold_tendencia_mensal_avancada`: Análise avançada com funções de janela

## 4. Métricas e Indicadores

| Métrica | Definição | Fórmula | Tabela | Interpretação |
|---------|-----------|---------|--------|---------------|
| Consumo Total por Estado | Soma do consumo por estado | SUM(consumo_mwm) | gold_consumo_por_estado | Comparação entre regiões |
| Média de Consumo por Estado | Média do consumo por estado | AVG(consumo_mwm) | gold_consumo_por_estado | Identificação de outliers |
| Tendência Mensal | Consumo por mês/ano | SUM/AVG(consumo_mwm) | gold_tendencia_mensal | Evolução temporal |
| Top Ramos | Ramos com maior consumo | SUM(consumo_mwm) LIMIT 10 | gold_top_ramos | Setores prioritários |
| Tendência Avançada | Variação mensal | LAG functions | gold_tendencia_mensal_avancada | Análise detalhada |

## 5. Monitoramento e Logging

### Sistema de Logging
- Implementado com a biblioteca `logging` do Python
- Registra timestamp, nome do pipeline, etapa, status, registros processados e erros

### Acesso aos Logs
- Através do driver do Spark
- Arquivos de log no sistema de arquivos

### Métricas de Monitoramento
- Tempo de execução por etapa
- Número de registros processados
- Número de erros encontrados
- Status geral do pipeline
