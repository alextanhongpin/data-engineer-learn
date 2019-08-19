# data-engineer-learn

Stuff to learn as a data engineer

## What are data engineers?

https://www.cio.com/article/3292983/what-is-a-data-engineer.html

> Data engineers are vital members of any enterprise data analytics team, responsible for managing, optimising, overseeing and monitoring data retrieval, storage and distribution throughout the organisation.

## Data engineer role

- __Generalist__: typically found in small teams or in small companies. Generalists are often responsible for every step of the data process, from managing data to analysing it.
- __Pipeline-centric__: Often found in midsize companies, pipeline-centric data engineers work alongside data scientists to help make use of the data they collect. Pipeline-centric data engineers need “in-depth knowledge of distributed systems and computer science”
- __Database-centric__: In larger organisations, while managing the flow of data is a full-time job, data engineers focus on analytics databases. Database-centric data engineers work with data warehouse across multiple databases and are responsible for developing table schemas.

## Data engineers responsibility

- Develop, construct, test and mantain architecture (how to develop one? Are there any basic design?)
- Align architecture with business requirements (what does this mean? Say, if we need to mine twitter, then the architecture is more on firing rest api to get the Twitter data. If we need to get data from the database, then we can use mysql binlog or ETL)
- Data acquisition (from different sources, CSV, SQL, rest api). Take a look at Presto?
- Develop data set processes? (Could be in the enricher pipeline)
- Use programming languages and tools (what are languages used? Scala and python. What are the tools used?)
- Identify ways to improve data reliability, efficiency and quality
- Conduct research for industry and business questions
- Use large data sets to question business issues (?aggregation of data provides summary)
- Deploy sophisticated analytics programs, machine learning and statistical methods). 
- Prepare data for predictive and prescriptive modelling (how? Feature selection? Remove null/fill up null values)
- Find hidden patterns using data (clustering?)
- Use data to discover tasks that can be automated
- Deliver updates to stakeholder based on analytics

## Data engineer skills

- Scala
- Apache spark
- Data warehouse
- Java
- Data modeling
- Apache Hadoop
- Linux
- Amazon web services 
- ETL 
- Big data analytics
- Software development


## Questions

- [What is the difference between data engineer and data scientist?](#difference)
- [What is a data pipeline?](#what-is-data-pipeline)
- [What are the phases in data pipeline](#phases-in-data-pipeline)
- [What are the different layers in data pipeline](#different-layers)
- How does a typical data pipeline looks like
- What steps are available?
- How to know if a job is completed?
- How to handle errors?
- [What are some considerations that needs to be taken when designing a data pipeline](#considerations)
- How to handle versioning of data?
- How to improve performance (using binary data such as protobuff/thrift, or maybe parquet)
- What is Presto, and other alternatives?
- High availability
- What are all the machine learning algorithms that can be applied to analyse a dataset? 
- data preparation steps
- How to build ETL (scalable, performant) pipelines?
- How to build connectors (http, rest, soap)
- What kind of sources and sinks are available?
- What kind of monitoring tools to look at?
- What is Inmon and Kimball approach for data warehousing?
- Star vs snowflake schemas
- What are the different types of data sources
    - server logs
        - server access logs. These contains one line per request made to the server from the app. Nginx access logs can be used.
        - server error logs. These contains all the server-side errors generated by your app.
    - App analytics logs
        - app events logs. These contains information about what actions users took in the app, e.g. when user update payment info, user click purchase button
        - app error logs: These contain information about errors in the app.
    - Database. SQL, no SQL. the single source of truth of the final state of the data.
    - Text file. CSV, Word, excel etc.
- How to perform autoscaling? There are times when there are spike requests. This requires the system to be scaled up when that time arrives. Some services might have less usage at midnight, and we need to find ways to scale down the service.

## <a id='phases-in-data-pipeline'>What are the phases in data pipeline?</a>

- __ingestion__: gathering the needed data
- __processing__: processing the data to get the result you want
- __storage__: this involves storing the end result for fast retrieval
- __access__: you will need to enable a tool or user to access the end results of the pipeline


## <a id='difference'> Difference between data scientist and data engineers</a>

> Data engineers are the plumbers building a data pipeline, while data scientists are the painters and storytellers, giving meaning to an otherwise static entity.

- Data engineers are more focused on building infrastructure and architecture of data generation. 
- Data scientist are focused rather on advanced mathematics and statistical analytics on that generated data.
- Data engineers and data scientist complements one another

References:
- https://blog.panoply.io/what-is-the-difference-between-a-data-engineer-and-a-data-scientist

## <a id='what-is-data-pipeline'>What is a data pipeline?</a>

A system that captures, organises, and routes data so that it can be used to gain insights. (How does it do that?)

## <a id='different-layers'>What are the different layers of the data pipeline?</a>

- __Authentication__: restricts access to the data set. 
- __Vault__: holds credentials for secure access to databases/secrets
- __Configuration data__: deals with the configuration of the preprocessing (?), may contain metadata on what to extract/filter/transform
- __Data validation__: validation of the schema
- __Enrichment__: pulls data from other sources to enrich the dataset (convert ids to denormalised data)
- __Extraction__: Selection of the data required for analytics
- __Transformation__: Normalizing of values (currency, date time etc)
- __Cleansing__: Removal of sensitive data, user compliance data, passwords, credit card number etc
- __Aggregation__: Aggregates the data, a.k.a map reduce
- __Storage__: stores the processed data in the databases/file storage
- __Archive__: for audit purposes

## <a id='considerations'>What are the things to consider when designing a data pipeline?</a>

- __Versioning__. Changes in schema (or formatting) on the data will require changes in the script that handles the processing, that would otherwise cause failures in the data pipeline and create data that is incorrect. To detect such changes, always log errors or changes when its made and alert the engineers.
- __Data validation__. Tricky, wrong validation can cause a lot of damage to the data too. E.g. date formatting that is not explicit, `1-2-1990` can be mistaken as either 1 February 1990 or 2 January 1990.
- __Cleansing__: Credit card numbers/passwords needs to be masked, sensitive user data needs to be removed

## Key skills for data engineers

- In-depth knowledge of SQL and other databases solution
- Data warehouse architecture and ETL tools, e.g. Redshift, Panoply. 


Data modelling Kimball



## Metadata

From configuration to tasks. We can pull the configuration metadata such as
- Components: `{“name”: “CopyData”, “type”: “CopyActivity”}`
- Tasks: copydata_1_20190819_00:00:00
    - The task will hold the following state:
        - status
        - invoked at datetime
        - attempts
        - errors
        - elapsed time
- API Rate Limiting: We can define the rate limiter configuration for each and every API Endpoints we have, say RateLimiter(10) // 10 requests per seconds. The limits are obtained from the API documentation. 

## Creating reusable transform layers

Transform layers: Create configuration files to define the input, and output. say we have a transformation layer that converts strings to int.
```python
def string_to_int(row):
row[‘age’] = int(row[‘age’])
return row

def transform(rows):
for row in rows:
row = string_to_int(row)
```
## How do we measure server efficiency
- monitoring for failures
- monitoring for duration (finding slow paths)
- monitoring cpu/memory usage

## How can we design to handle 1k+ ThirdParty SQL based data sources

## How can we design to handle undefined CSV as a data source?
- parse and validate the format
- check the headers
- convert null values to defaults based on the domain


## ETL logging
- log relevant steps that occur before, during and after the execution of ETL
- start/end time gives the duration
- status (started/running/success/failure)
- errors and exceptions
- audit information
- testing and debugging infromation for troubleshooting

## Building a data warehouse


## References

https://multithreaded.stitchfix.com/blog/2016/03/16/engineers-shouldnt-write-etl/
https://www.kdnuggets.com/2019/01/role-data-engineer-changing.html
https://www.dataquest.io/blog/what-is-a-data-engineer/
https://hbr.org/2012/10/data-scientist-the-sexiest-job-of-the-21st-century?source=post_page---------------------------
https://hackernoon.com/the-ai-hierarchy-of-needs-18f111fcc007?source=post_page---------------------------
https://engineering.linkedin.com/distributed-systems/log-what-every-software-engineer-should-know-about-real-time-datas-unifying
https://medium.com/@samson_hu/building-analytics-at-500px-92e9a7005c83
https://www.alooma.com/blog/what-is-a-data-pipeline
https://www.jesse-anderson.com/2019/01/the-three-components-of-a-big-data-data-pipeline/
https://www.dremio.com/what-is-a-data-pipeline/
