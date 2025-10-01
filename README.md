# Monitoramento de Vendas – BigQuery + Looker Studio 
Integração de dados no BigQuery com visualização em Looker Studio, aplicando consultas SQL para monitoramento de faturamento e indicadores de performance financeira.

Este projeto tem como objetivo criar um **painel interativo de monitoramento de vendas** utilizando:  

- **Google BigQuery** → para consultas SQL e manipulação de dados.  
- **Looker Studio (Google Data Studio)** → para visualização dinâmica e interativa.  

Os dados utilizados são fictícios, retirados do Kaggle, e simulam o cenário de vendas de um e-commerce.  

---

## 🗂 Estrutura do Conjunto de Dados  

O dataset se chama **`vendas`** e contém as seguintes tabelas:  

### 📌 Categoria  
- **id** → identificador da categoria.  
- **name** → nome da categoria.  

### 📌 Ordens (Pedidos)  
- **id** → identificador do pedido.  
- **created_at** → data e hora em que o pedido foi feito.  
- **custom_id** → identificador do cliente (não utilizado nas análises).  
- **status** → status do pedido (ex.: entregue, cancelado, pagamento pendente, carrinho).  

### 📌 Produto  
- **id** → identificador do produto.  
- **name** → nome do produto.  
- **price** → preço unitário do produto.  
- **category_id** → referência à categoria do produto.  

### 📌 Itens (Vendas)  
- **id** → identificador da venda (linha).  
- **order_id** → identificador do pedido (ligado à tabela de ordens).  
- **product_id** → identificador do produto (ligado à tabela de produtos).  
- **quantity** → quantidade vendida daquele produto no pedido.  
- **total_price** → valor total daquela venda (quantidade × preço).  

---

## 🔎 Consultas SQL (BigQuery)  

A base do projeto foi construída a partir de consultas SQL que integram múltiplas tabelas para gerar informações consolidadas.  

Exemplo de consulta principal:  

```sql
--- objetivo: visão geral das vendas
SELECT 
    v.id AS venda_id,
    v.order_id,
    o.created_at,
    DATE(o.created_at) AS order_date,
    TIME(o.created_at) AS order_time,
    v.quantity,
    v.total_price,
    o.status,
    p.name AS nome_produto,
    c.name AS nome_categoria
FROM `e-commerce-420222.vendas.itens` AS v  -- Itens = Vendas
LEFT JOIN `e-commerce-420222.vendas.Ordens` AS o
    ON v.order_id = o.id
LEFT JOIN `e-commerce-420222.vendas.Produto` AS p
    ON v.product_id = p.id
LEFT JOIN `e-commerce-420222.vendas.Categoria` AS c
    ON p.category_id = c.id;

Essa query unifica vendas, pedidos, produtos e categorias, permitindo alimentar diretamente o Looker Studio e manter o dashboard sempre atualizado automaticamente.

📈 Painel no Looker Studio

O painel construído no Looker Studio permite diferentes formas de análise:

🔍 Busca por pedido

Ao digitar o número de um pedido, todos os gráficos são atualizados automaticamente para mostrar somente os dados daquele pedido.

📦 Quantidade de produtos e 💰 Faturamento total

Indicadores principais que mostram a quantidade de itens vendidos e o valor total de faturamento acumulado.

✅ Filtros por status

Permite acompanhar pedidos entregues, pendentes, cancelados e em carrinho.

📅 Filtros por mês e ano

Facilitam a análise temporal, permitindo selecionar períodos específicos.

🏷 Faturamento por categoria

Mostra quais categorias são mais relevantes em faturamento.

📊 Faturamento ao longo dos meses

Ajuda a identificar sazonalidade, promoções e quedas de vendas.

📌 Top produtos vendidos

Lista os produtos que mais geraram receita e quantidade vendida.

📆 Faturamento por ano

Permite comparar anos diferentes para avaliar o crescimento ou retração do negócio.

💡 Insights Possíveis

Com esse painel é possível extrair diversos insights estratégicos para o negócio, como:

Identificar quais categorias geram maior faturamento (ex.: Celulares e Eletrodomésticos).

Descobrir os produtos mais vendidos e direcionar estratégias de marketing.

Acompanhar a evolução do faturamento ao longo dos meses e detectar quedas ou picos de vendas.

Comparar o desempenho ano a ano e avaliar o crescimento ou retração.

Monitorar status de pedidos e identificar gargalos (ex.: muitos pedidos pendentes ou cancelados).

Analisar o impacto de sazonalidade em determinados períodos (ex.: promoções ou datas comemorativas).

🚀 Objetivo

O objetivo principal é fornecer uma ferramenta de monitoramento de vendas em tempo real, permitindo que gestores e analistas consigam explorar os dados de forma interativa, desde o detalhe de um pedido específico até a visão geral da empresa.
