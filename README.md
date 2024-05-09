# Create-External-Table-in-Azure-Synapse

## 1. create a datasource

CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'Xyz@123#$';
CREATE DATABASE SCOPED CREDENTIAL SasToken
WITH IDENTITY='SHARED ACCESS SIGNATURE'
, SECRET =
'sv=2020-08-04&ss=b&srt=sco&sp=rwdlacx&se=2022-05-27T02:39:58Z&st=2022-05-26T18:39:58Z&spr=h
ttps&sig=cFumoIzqc0FeIWeIlLuayon6EO5pfWs7uWV12M4iJ2c%3D';
CREATE EXTERNAL DATA SOURCE ExtDataSrc
WITH (LOCATION = 'https://trendytechsa101.dfs.core.windows.net/data',
CREDENTIAL = SasToken)

## 2. create a file format

CREATE EXTERNAL FILE FORMAT TextFileFormat WITH (
FORMAT_TYPE = DELIMITEDTEXT,
FORMAT_OPTIONS (
FIELD_TERMINATOR = ',',
FIRST_ROW = 2))

## 3. create external table

CREATE EXTERNAL TABLE orders (
order_id bigint,
order_date nvarchar(4000),
customer_id bigint,
order_status nvarchar(4000)
)
WITH (
LOCATION = 'orders.csv',
DATA_SOURCE = ExtDataSrc,
FILE_FORMAT = TextFileFormat
)

GO
