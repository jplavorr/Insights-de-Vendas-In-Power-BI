# Modelagem de Dados com Power BI 

## Introdução

Esse é um projeto para praticar modelagem de dados com Power BI.

AtliQ é uma empresa é uma empresa que fornece hardware de computador e periféricos para muitos clientes através da Índia. Esse projeto vem com propósito de desbloquear insights de vendas que não eram visíveis para uma equipe de vendas. Automatizá-los para reduzir o tempo manual na coleta de dados. Definindo um star schema e escrevendo o pipeline ETL, os dados baseados em arquivos sql que será transferido para o Power BI para limpeza e dai criar um dashboard para suporte e decisões futuras.
## Data Schema e Pipeline ETL 

### Data Schema

A empresa está crescendo bastante no mercado e precisa de ajuda para rastrear suas vendas e ter insights sobre seu negócio.

Até então, as vendas eram passadas pelos gerentes regionais na Índia, no qual informavam sobre as vendas e o lucro previsto para aquela região. Fazendo uso de um banco de dados MySQL, temos o seguinte modelo:

- Star Schema: **sales transactions**, e 
- Demais Schemas: **sales customers**, **sales markets**, **sales products** e **sales date**.

![Star Schema](Imagens/sql_database.png)

Com os dados provenientes da empresa, iremos fazer um dashboard automatizado em Power BI para suporte e decisões sobre a empresa de forma otimizada.  

### ETL Pipeline

O ETL pipeline extrai os dados do arquivo `db_dump.sql` e produz um dashboard utilizando o `ETL_mysql.pbix`, como você pode observar abaixo

![Pipeline ETL](Imagens/ETL.png)

## Como executar

### Pré-requisitos

Se você deseja executar esse projeto em sua máquina, você deve finalizar os seguintes passos primeiro.

- Instalar `MySQL`
- Instalar `PowerBI` em sua máquina

### Instruções
1. Criar schema no MySQL
2. Importar o arquivo `db_dump.sql` nessa schema.
3. Analisar como estão os dados, se é preciso ser feita alguma limpeza.
4. Importar os dados no Power BI, importando como uma base de dados MySQL.
5. Fazer a limpeza que será descrita abaixo e criar o dashboard de vendas referente ao arquivo `ETL_mysql.pbix`.

### Análise de Dados usando SQL
Abaixo vão estar Querys que serão parte da nossa análise em power bi. Vamos usar essas Querys com dois propósitos, procurar saber se os nossos dados precisam de limpeza e para confirmar que a análise com power bi condiz com a análise em MySQL.

1. Mostre todas informações da tabela customers

    `SELECT * FROM customers;`

2. Mostre o total de linhas na tabela customers

    `SELECT count(*) FROM customers;`

3. Mostre as transações para o mercado de Chennai (O código de mercado para Chennai é Mark001)

    `SELECT * FROM transactions where market_code='Mark001';`

4. Mostre os produtos distintos vendidos em Chennai

    `SELECT distinct product_code FROM transactions where market_code='Mark001';`

5. Mostre as transações feitas em Dólares

    `SELECT * from transactions where currency="USD"`

6. Mostre as transações em 2020 usando o método join com a tabela data

    `SELECT transactions.*, date.* FROM transactions INNER JOIN date ON transactions.order_date=date.date where date.year=2020;`

7. Mostre toda receita do ano 2020

    `SELECT SUM(transactions.sales_amount) FROM transactions INNER JOIN date ON transactions.order_date=date.date where date.year=2020 and transactions.currency="INR\r" or transactions.currency="USD\r";`
	
8. Mostre toda receita do ano 2020 no mês de janeiro

    `SELECT SUM(transactions.sales_amount) FROM transactions INNER JOIN date ON transactions.order_date=date.date where date.year=2020 and and date.month_name="January" and (transactions.currency="INR\r" or transactions.currency="USD\r");`

9. Mostre toda receita do ano 2020 na região de Chennai

    `SELECT SUM(transactions.sales_amount) FROM transactions INNER JOIN date ON transactions.order_date=date.date where date.year=2020
and transactions.market_code="Mark001";`


Análise de Dados Usando Power BI
============================
Queremos agora corrigir problemas que encontramos análisando os dados com MySQL.

1. Fórmula para retirar valores indevidos da venda.

`= Table.SelectRows(sales_transactions, each ([sales_amount] <> -1 and [sales_amount] <> 0))`

2. Fórmula para criar a coluna new_sales_amount

`= Table.AddColumn(#"Cleanup Currency", "new_sales_amount", each if [currency] ="USD#(cr)" then [sales_amount]*74 else [sales_amount])`

3. Mudar o tipo da coluna new_sales_amount

`= Table.TransformColumnTypes(#"Added new_sales_amount with all currencys in INR",{{"new_sales_amount", type number}})`

4. Selecione cy_date ao mudar seu formato para 'mmm yy'

- [Dashboard com as análises realizadas](https://app.powerbi.com/groups/me/reports/776902f1-5a56-4ff3-ad05-79430683cb2a/ReportSection).

## Referencias: 

- [Data Analysis Projects do canal codebasics](https://www.youtube.com/playlist?list=PLeo1K3hjS3utcb9nKtanhcn8jd2E0Hp9b).
