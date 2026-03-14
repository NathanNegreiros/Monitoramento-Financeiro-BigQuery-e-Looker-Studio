# Monitoramento de Vendas — BigQuery e Looker Studio

Projeto de análise e monitoramento de vendas desenvolvido com consultas em SQL no Google BigQuery e visualização de dados no Looker Studio.

O objetivo foi integrar diferentes tabelas de um dataset de e-commerce para gerar uma visão consolidada de vendas, permitindo acompanhar indicadores de desempenho, analisar padrões de faturamento e identificar oportunidades de negócio.

Os dados utilizados são fictícios e foram obtidos no Kaggle para simular um ambiente real de análise de dados em e-commerce.

---

## Objetivo do Projeto

Construir um painel interativo capaz de acompanhar o desempenho de vendas e permitir diferentes tipos de análise, como evolução do faturamento ao longo do tempo, produtos mais vendidos e desempenho por categoria.

A proposta foi simular um cenário de monitoramento utilizado por equipes de negócio e análise de dados para apoiar a tomada de decisão.

---

## Ferramentas Utilizadas

- SQL  
- Google BigQuery  
- Looker Studio  

---

## Estrutura do Conjunto de Dados

O dataset utilizado se chama **vendas** e está organizado em quatro tabelas principais.

### Categoria

Contém as categorias de produtos disponíveis.

Campos principais:

- id — identificador da categoria  
- name — nome da categoria  

### Ordens (Pedidos)

Tabela que registra os pedidos realizados na plataforma.

Campos principais:

- id — identificador do pedido  
- created_at — data e hora da criação do pedido  
- custom_id — identificador do cliente  
- status — status do pedido (entregue, cancelado, pendente, carrinho)  

### Produto

Tabela que contém informações sobre os produtos vendidos.

Campos principais:

- id — identificador do produto  
- name — nome do produto  
- price — preço unitário  
- category_id — categoria associada ao produto  

### Itens (Vendas)

Tabela que representa as vendas realizadas em cada pedido.

Campos principais:

- id — identificador da venda  
- order_id — identificador do pedido  
- product_id — identificador do produto  
- quantity — quantidade vendida  
- total_price — valor total da venda  

---

## Consulta SQL

A base analítica do projeto foi construída a partir da integração das tabelas de pedidos, produtos e categorias.

### Consulta principal utilizada

```sql
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
FROM `e-commerce-420222.vendas.itens` AS v
JOIN `e-commerce-420222.vendas.Ordens` AS o
    ON v.order_id = o.id
JOIN `e-commerce-420222.vendas.Produto` AS p
    ON v.product_id = p.id
JOIN `e-commerce-420222.vendas.Categoria` AS c
    ON p.category_id = c.id;
```

Essa consulta consolida as informações necessárias para análise de vendas, permitindo relacionar pedidos, produtos e categorias em uma única estrutura de dados.

---

## Dashboard no Looker Studio

O painel foi desenvolvido para facilitar a exploração dos dados e permitir diferentes perspectivas de análise.

Principais funcionalidades do dashboard:

- busca por número de pedido com atualização automática dos gráficos  
- indicadores de quantidade total de produtos vendidos e faturamento  
- filtros por status de pedido  
- filtros por mês e ano  
- análise de faturamento por categoria  
- evolução do faturamento ao longo do tempo  
- identificação dos produtos mais vendidos  
- comparação de faturamento entre anos  

### Dashboard interativo

https://lookerstudio.google.com/u/0/reporting/edb24dc7-37c4-4faf-9aad-b249303a2d19/page/xFDaF

### Visualização do Dashboard

![Dashboard](image.png)

---

## Possíveis Insights

A partir do painel é possível identificar diferentes padrões de negócio, como:

- categorias com maior participação no faturamento  
- produtos responsáveis pelo maior volume de vendas  
- evolução do faturamento ao longo dos meses  
- possíveis efeitos de sazonalidade nas vendas  
- comparação de desempenho entre diferentes períodos  
- monitoramento de pedidos cancelados ou pendentes  

Essas análises ajudam a identificar oportunidades de crescimento, ajustes de estratégia comercial e melhorias na gestão operacional.
