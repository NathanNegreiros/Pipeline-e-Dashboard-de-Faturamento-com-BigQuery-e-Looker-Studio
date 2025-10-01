# Monitoramento de Vendas – BigQuery + Looker Studio  

Este projeto tem como objetivo criar um **painel interativo de monitoramento de vendas** utilizando:  

- **Google BigQuery** → para consultas SQL e manipulação de dados.  
- **Looker Studio (Google Data Studio)** → para visualização dinâmica e interativa.  

Os dados utilizados são fictícios, retirados do Kaggle, simulando o cenário de vendas de um e-commerce.  

---

## Estrutura do Conjunto de Dados  

O dataset se chama **`vendas`** e contém as seguintes tabelas:  

### Tabela: Categoria  
- **id** → identificador da categoria  
- **name** → nome da categoria  

### Tabela: Ordens (Pedidos)  
- **id** → identificador do pedido  
- **created_at** → data e hora em que o pedido foi feito  
- **custom_id** → identificador do cliente (não utilizado nas análises)  
- **status** → status do pedido (ex.: entregue, cancelado, pagamento pendente, carrinho)  

### Tabela: Produto  
- **id** → identificador do produto  
- **name** → nome do produto  
- **price** → preço unitário do produto  
- **category_id** → referência à categoria do produto  

### Tabela: Itens (Vendas)  
- **id** → identificador da venda (linha)  
- **order_id** → identificador do pedido (ligado à tabela de ordens)  
- **product_id** → identificador do produto (ligado à tabela de produtos)  
- **quantity** → quantidade vendida daquele produto no pedido  
- **total_price** → valor total daquela venda (quantidade × preço)  

---

## Consulta SQL (BigQuery)  

A base do projeto foi construída a partir de consultas SQL que integram múltiplas tabelas para gerar informações consolidadas.  

### Exemplo de consulta principal:  

```sql
-- Visão geral das vendas
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
```


---

## Painel no Looker Studio  

O painel construído no Looker Studio permite diferentes formas de análise:  

- **Busca por pedido** → ao digitar o número de um pedido, todos os gráficos são atualizados automaticamente.  
- **Quantidade de produtos e Faturamento total** → indicadores principais que mostram a quantidade de itens vendidos e o faturamento acumulado.  
- **Filtros por status** → acompanhamento de pedidos entregues, pendentes, cancelados e em carrinho.  
- **Filtros por mês e ano** → análise temporal de períodos específicos.  
- **Faturamento por categoria** → categorias mais relevantes em termos de receita.  
- **Faturamento ao longo dos meses** → identificação de sazonalidade, promoções e quedas de vendas.  
- **Top produtos vendidos** → produtos que mais geraram receita e quantidade vendida.  
- **Faturamento por ano** → comparação entre anos para avaliar crescimento ou retração.  

### Imagem e link do Dashboard:  
![Imagem do Dash](imagem.png)

---
https://lookerstudio.google.com/u/0/reporting/edb24dc7-37c4-4faf-9aad-b249303a2d19/page/xFDaF

## Possíveis Insights  

Com esse painel é possível extrair diversos insights estratégicos, como:  

- Identificar quais categorias geram maior faturamento.  
- Descobrir os produtos mais vendidos e direcionar campanhas de marketing.  
- Acompanhar a evolução do faturamento ao longo dos meses.  
- Comparar o desempenho ano a ano e avaliar crescimento ou retração.  
- Monitorar status de pedidos e identificar gargalos (ex.: pedidos pendentes ou cancelados).  
- Analisar impactos de sazonalidade em datas específicas (promoções ou datas comemorativas).  

---

## Objetivo  

Fornecer uma ferramenta de monitoramento de vendas em tempo real, permitindo que gestores e analistas explorem os dados de forma interativa — desde o detalhe de um pedido específico até a visão consolidada da empresa.  

---
