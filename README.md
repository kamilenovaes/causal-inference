# Análise de Impacto Causal de Campanha de Marketing

> **Nota:** Este é um notebook para fins de estudo e aprendizado, podendo conter erros de código e análise.

![Status do Projeto](https://img.shields.io/badge/status-concluído-brightgreen)
![Python](https://img.shields.io/badge/Python-3.9%2B-blue)
![Libraries](https://img.shields.io/badge/libraries-pandas%2C%20dowhy%2C%20scikit--learn-orange)

## 1. Objetivo do Projeto

Este projeto visa mensurar o impacto real de uma campanha de marketing no comportamento de compra dos clientes. A análise vai além da correlação simples, que pode ser enganosa devido ao viés de seleção (ex: a campanha ser ofertada a clientes que já gastam mais).

O objetivo é usar técnicas de **inferência causal** para isolar e quantificar o efeito puro da campanha, respondendo à pergunta: "A campanha *causou* um aumento nas vendas, ou ela apenas foi aceita por clientes que já comprariam de qualquer forma?"

## 2. Metodologia Aplicada

* **Fonte de Dados**: [Customer Personality Analysis](https://www.kaggle.com/datasets/imakash3011/customer-personality-analysis) do Kaggle.

* **ETL (Extração, Transformação e Carga)**: O processo inicial incluiu limpeza de dados nulos, engenharia de features para criar variáveis como `Age`, `Customer_Tenure` e `Total_Mnt`, e a padronização das escalas numéricas para otimizar o modelo.

* **Técnica Causal**: A análise foi centrada no método de quasi-experimentação **Propensity Score Matching (PSM)**, implementado com a biblioteca `DoWhy`. Este método busca criar um grupo de controle estatisticamente comparável ao grupo de tratamento, mitigando o viés de seleção. A robustez foi verificada usando Regressão Linear como método de estimação alternativo.

## 3. Resultados e Conclusão

A análise revelou uma história complexa e estrategicamente valiosa sobre o impacto da campanha.

### Principais Descobertas

A tabela abaixo resume os efeitos causais estimados (ATE) da campanha em diferentes métricas:

| Análise                        | Resultado Medido (Outcome) | Método de Estimação       | Efeito Causal Estimado (ATE) |
| ------------------------------ | -------------------------- | ------------------------- | ---------------------------- |
| **Gastos Totais (Principal)** | `Total_Mnt`                | Propensity Score Matching | **R$ 38.33** |
| Compras em Loja                | `NumStorePurchases`        | Propensity Score Matching | **-0.50 compras** |
| Compras na Web                 | `NumWebPurchases`          | Propensity Score Matching | **+1.12 compras** |
| **Gastos Totais (Validação)** | `Total_Mnt`                | Regressão Linear          | **R$ 158.29** |

### Interpretação e Conclusão

Os resultados indicam que a campanha teve um **impacto causal modesto no aumento do gasto total** (aproximadamente R$ 38, segundo o método mais robusto de PSM), mas provocou uma **profunda mudança no comportamento do consumidor**.

O efeito mais significativo foi a **migração de transações do canal físico para o online**. A campanha causou uma leve queda nas compras em loja, que foi mais do que compensada por um ganho expressivo no e-commerce. A discrepância entre os métodos de PSM e Regressão Linear reforça a complexidade dos dados e sugere que o resultado mais conservador (R$ 38.33) é a estimativa mais confiável, por fazer menos suposições sobre a linearidade das relações entre as variáveis.