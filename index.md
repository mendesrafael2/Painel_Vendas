# Painel de Vendas
<p>O presente trabalho tem como objetivo desenvolver um painel para análise de vendas utilizando os softwares  <b>Power BI</b> e <b>SQL Server</b>.</p>

<p>Para implementação do painel foi utilizada a base de dados <a href="https://docs.microsoft.com/pt-br/sql/samples/adventureworks-install-configure?view=sql-server-ver15&tabs=ssms">Adventure Works</a>, essa base é um banco de dados de exemplo criado pela Microsoft que simula uma empresa multinacional fictícia de manufatura chamada Adventure Works. Esse banco de dados possui quase 20.000 clientes, mais de 70.000 pedidos e 500 produtos.</p>

<p>Inicialmente foram criados arquivos no formato csv utilizando algumas tabelas do banco de dados. Para tal foram realizadas algumas consultas utilizando o SQL Server:</p>

<p>A seguir são apresentados os nomes das tabelas e as respectivas consultas realizadas.</p>

<p><b>DIM_Calendar:</b></p>
  
```html
  SELECT
  [DateKey]  AS [Id Data],
  [FullDateAlternateKey] AS Data,
  [EnglishDayNameOfWeek] AS Dia,
  [EnglishMonthName] AS Mês,
  Left([EnglishMonthName], 3) AS [Mês Abreviado],  
  [MonthNumberOfYear] AS [Mês Nr],
  [CalendarQuarter] AS Quarter,
  [CalendarYear] AS Ano
  
FROM
 [AdventureWorksDW2019].[dbo].[DimDate]
WHERE 
  CalendarYear >= 2019
```
<p><b>DIM_Customers:</b></p>
  
```html
  SELECT
  c.customerkey AS [Id Consumidor],
  c.firstname AS [Primeiro Nome],
  c.lastname AS [Último Nome],
  c.firstname + ' ' + lastname AS [Nome Completo],
  CASE c.gender WHEN 'M' THEN 'Male' WHEN 'F' THEN 'Female' END AS Gênero,
  c.datefirstpurchase AS [Data da Primeira Compra],
  g.city AS [Cidade do Consumidor] 
FROM
  [AdventureWorksDW2019].[dbo].[DimCustomer] as c
  LEFT JOIN dbo.dimgeography AS g ON g.geographykey = c.geographykey
ORDER BY
  [Id Consumidor] ASC
```

<p><b>DIM_Calendar:</b></p>  
  
```html
  SELECT
  c.customerkey AS [Id Consumidor],
  c.firstname AS [Primeiro Nome],
  c.lastname AS [Último Nome],
  c.firstname + ' ' + lastname AS [Nome Completo],
  CASE c.gender WHEN 'M' THEN 'Male' WHEN 'F' THEN 'Female' END AS Gênero,
  c.datefirstpurchase AS [Data da Primeira Compra],
  g.city AS [Cidade do Consumidor] 
FROM
  [AdventureWorksDW2019].[dbo].[DimCustomer] as c
  LEFT JOIN dbo.dimgeography AS g ON g.geographykey = c.geographykey
ORDER BY 
  [Id Consumidor] ASC 
```
  
<p><b>DIM_Products:</b></p>
  
```html
  SELECT 
  p.[ProductKey] AS [Id Produto],
  p.[ProductAlternateKey] AS [Código do Produto],  
  p.[EnglishProductName] AS [Nome do Produto], 
  ps.EnglishProductSubcategoryName AS [Sub Categoria], 
  pc.EnglishProductCategoryName AS [Categoria do Produto],  
  p.[Color] AS [Cor do Produto],  
  p.[Size] AS [Tamanho do Produto], 
  p.[ProductLine] AS [Linha do Produto], 
  p.[ModelName] AS [Nome do Produto], 
  p.[EnglishDescription] AS [Descrição do Produto], 
  ISNULL (p.Status, 'Outdated') AS [Status Produto] 
FROM 
  [AdventureWorksDW2019].[dbo].[DimProduct] as p
  LEFT JOIN dbo.DimProductSubcategory AS ps ON ps.ProductSubcategoryKey = p.ProductSubcategoryKey 
  LEFT JOIN dbo.DimProductCategory AS pc ON ps.ProductCategoryKey = pc.ProductCategoryKey 
ORDER BY 
  p.[ProductKey] ASC
```

<p><b>FACT_InternetSales:</b></p>
  
```html
  SELECT 
  [ProductKey] AS[Id Produto], 
  [OrderDateKey], 
  [DueDateKey], 
  [ShipDateKey], 
  [CustomerKey] AS [Id Consumidor] , 
  [SalesOrderNumber], 
  [SalesAmount]
FROM 
  [AdventureWorksDW2019].[dbo].[FactInternetSales]
WHERE 
  LEFT (OrderDateKey, 4) >= YEAR(GETDATE()) -2
ORDER BY
  OrderDateKey ASC
```
  
<p> Os arquivos .csv gerados a partir da consultas apresentadas foram utilizados como fonte de dados para o painel apresentado abaixo.</p>
  
<p>O painel apresenta uma visão geral das vendas utilizando algumas visualizações de dados bem como alguns recursos de interatividade para auxiliar o usuário em possíveis análises.</p>
  
<a target="_blank" rel="noopener noreferrer" href="https://app.powerbi.com/view?r=eyJrIjoiOTY5NGE4MzgtNjMwNy00NjE0LWIzYzYtNGViNTFmNDUyOGIzIiwidCI6IjI4ZjBlOGY1LWVlNzUtNDIxZC1iYWIxLTM3YTlmMzgxZDQ3ZSJ9&pageName=ReportSection" >
<b>Click aqui para acessar o painel:</b> </a>

<p align="center">
 <img  src="https://raw.githubusercontent.com/mendesrafael2/Painel_Vendas/main/Version_1_page-0001.jpg" alt="some text" width=900 height=500>
 <br> <b>Figura 1. Painel de Vendas</b>
</p>
