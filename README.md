# Snowpipe Streaming and Dynamic Tables for Real-Time Ingestion (CDC Use Case)
## 1. Overview
**This guide will take you through a scenario of using Snowflake's Snowpipe Streaming to ingest a simulated stream, then utilize Dynamic tables to transform and prepare the raw ingested JSON payloads into ready-for-analytics datasets. These are two of Snowflake's powerful Data Engineering innovations for ingestion and transformation.**

**A simulated streaming datafeed will be generated for this exercise, using a java client running on your desktop, that implements the Snowpipe Streaming SDK. The simulated datafeed will be Stock Limit Orders, with new, changed, and cancelled orders represented as RDBMS transactions logs captured from INSERT, UPDATE, and DELETE database events. These events will be transmitted as JSON payloads and land into a Snowflake table with a variant data column. The simulation will be high-volume, first starting at 1 million transactions in seconds and secondly as steady stream. Finally, data will contain sensitive fields, so our process will have extra protections for this.**

![image](https://github.com/King4424/Dynamic-Tables-Ingestion/assets/121480992/35c4f527-5f05-406a-ae1f-3e11193e359b)

1.**Java program**: This program acts as a mock database, simulating real-time data changes.

2.**Streaming agent**: This agent captures the change events from the Java program’s database transactions. These events are represented in JSON format.

3.**Snowpipe**: The streaming agent sends the change events to Snowflake using Snowpipe. Snowpipe is a cloud-based data integration service that ingests data into Snowflake from various sources.

4.**All-events landing table**: The change events are initially stored in an “all-events” landing table. This table is a temporary staging area for all the incoming data.

5.**Dynamic tables**: Snowflake automatically creates dynamic tables based on the schema of the data in the landing table. These dynamic tables have the same structure as the landing table, but they are partitioned by date and time.

6.**Views**: Secure views are created on top of the dynamic tables. These views can be used to filter and transform the data before it is loaded into other tables.

7.**Dimension tables (SCD)**: Slowly changing dimension (SCD) tables are used to store dimension data, such as customer or product information. These tables are updated with the latest changes from the change events.

8.**Summary tables**: Summary tables are created to aggregate the data from the dimension and fact tables. These tables can be used for reporting and analysis.
