# Projeto de Análise Exploratória - Olist E-Commerce

 Análise e pré-processamento de dados do e-commerce brasileiro para identificar fatores que afetam a satisfação do cliente
 
---

## Integrantes

- **Marcus Mikael Rodrigues Vieira**
- **Marcos Andre dos Santos Soares**

---

## Base de Dados

**Dataset:** [Olist Brazilian E-Commerce Dataset](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce) (Kaggle)

### Datasets Utilizados:
- `olist_orders_dataset.csv` - 99.441 pedidos
- `olist_order_items_dataset.csv` - 112.650 itens
- `olist_products_dataset.csv` - 32.951 produtos

**Dataset final:** 112.624 registros × 43 colunas

---

## Objetivo do Projeto

Analisar e pré-processar dados do e-commerce brasileiro **Olist** para identificar padrões que influenciam a experiência e satisfação do cliente, com foco em:

-  Atrasos de entrega
-  Diferenças de preço e frete
-  Categorias de produtos problemáticas
-  Variações no tempo de processamento e envio

**Tema Central:** *"O que afeta a experiência e satisfação do cliente no e-commerce brasileiro?"*

---

##  Processo de Tratamento dos Dados

### 1️ **Exploração Inicial (EDA)**
- Análise de estatísticas descritivas
- Matriz de correlação (identificou peso × frete: 0.61)
- Visualizações: histogramas, boxplots, heatmaps

### 2️ **Limpeza de Dados**
| Etapa | Ação | Resultado |
|-------|------|-----------|
| Duplicatas | Verificação | 0 encontradas ✅ |
| Inconsistências | Remoção | 8 registros (peso=0) |
| Valores nulos | Tratamento | 6.359 tratados |
| Outliers | Análise IQR | Mantidos (naturais) |

**Decisões Importantes:**
-  **Nulos em datas de entrega mantidos** - representam pedidos não entregues/cancelados
-  **Outliers preservados** - produtos de R$6.735 e 40kg são reais, não erros

### 3️ **Conversão de Tipos**
- 6 colunas de datas → `datetime64`
- `order_item_id` → `Int64`

### 4️ **Tratamento Categórico**
- Categorias: 50 → 21 (top 20 + "other")
- One-Hot Encoding: 21 colunas binárias criadas

### 5️ **Normalização**
- **StandardScaler** (Z-score): média=0, desvio=1
- **MinMaxScaler** (alternativa): valores [0,1]

### 6️ **Seleção de Atributos**
- Variance Threshold: 0 removidas (todas variáveis têm variação adequada)
- Correlação >0.90: 0 removidas (sem multicolinearidade)
- Filtros simples: 5 IDs removidos

### 7️ **Feature Engineering** ⭐
Criadas **6 novas features**:

| Feature | Descrição | Tipo |
|---------|-----------|------|
| `delivery_delay_days` | Dias de atraso na entrega | Numérica |
| `freight_ratio` | Relação frete/preço | Numérica |
| `product_volume_cm3` | Volume do produto (cm³) | Numérica |
| `delivered_late` | Entrega atrasada? | Binária (0/1) |
| `order_sent` | Pedido enviado? | Booleana |
| `delivered` | Pedido entregue? | Booleana |

---

##  Principais Desafios Encontrados

### 1. **Valores Nulos em Datas de Entrega**
**Problema:** 2.454 pedidos sem data de entrega  
**Solução:** Mantidos propositalmente - representam pedidos não entregues/cancelados  
**Justificativa:** São informação válida para análise de taxa de entrega

### 2. **Outliers Naturais**
**Problema:** 76.653 outliers detectados em preço, peso, frete  
**Solução:** Mantidos após análise  
**Justificativa:** 
- Produtos de R$6.735 (notebooks) e 40kg (geladeiras) são reais
- Remover causaria perda dos dados
- Outliers têm valor examinador importante

### 3. **Categorias Raras**
**Problema:** 50+ categorias, muitas com poucos produtos  
**Solução:** Agrupar top 20 + outros("other")  

### 4. **Merge de 3 Datasets**
**Problema:** Datasets com valores ausentes ao unir  
**Solução:** Left join preservando todos os pedidos + tratamento específico por coluna

### 5. **Tomar devidas decisões**
**Problema:** Decisões erradas podia causar problema na analise

---

##  Principais Conclusões

###  **Sobre Atrasos:**
- Taxa geral de atraso: **6,6%**
- Produtos pesados (>5kg) tende a atrasar mais
- Categorias problemáticas: Móveis (15,3%), Eletrodomésticos (11,7%)

###  **Sobre Frete:**
- **Peso é o principal determinante do frete** (correlação: 0.61)
- Frete representa em média **32% do preço**
- 16.8% dos produtos têm frete > 50% do preço (problemático)

###  **Sobre Categorias:**
- Top 20 categorias cobrem **85%** dos produtos
- Móveis e eletrodomésticos concentram problemas logísticos
- Eletrônicos pequenos têm melhor performance (4% atraso)

###  **Qualidade dos Dados:**
- **99,3%** dos dados originais preservados
- **99,85%** de completude após tratamento
- 0 duplicatas, 0 inconsistências
- Alguns nulos foram mantidos
- Outliers são naturais, não erros
