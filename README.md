# Modelagem de Dados com Power BI 

## Introdução

Esse é um projeto para praticar modelagem de dados com Power BI.

AtliQ é uma empresa é uma empresa que fornece hardware de computador e periféricos para muitos clientes através da Índia. Esse projeto vem com propósito de desbloquear insights de vendas que não eram visíveis para uma equipe de vendas. Automatizá-los para reduzir o tempo manual na coleta de dados. Definindo um star schema e escrevendo o pipeline ETL, os dados baseados em arquivos sql que será transferido para o Power BI para limpeza e dai criar um dashboard para suporte e decisões futuras.
## Data Schema e Pipeline ETL 

### Data Schema

AtliQ está crescendo bastante no mercado e precisa de ajuda para rastrear suas vendas e ter insights sobre seu negócio.

Até então, as vendas eram passadas pelos gerentes regionais na Índia, no qual informavam sobre as vendas e o lucro previsto para aquela região. Fazendo uso de um banco de dados MySQL, temos o seguinte modelo:

- Uma tabela fato: **songplays**, e 
- Quatro tabelas dimensões: **users**, **songs**, **artists** and **time**.

![Star Schema](images/star_schema.png)
