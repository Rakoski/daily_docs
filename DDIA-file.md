# Notes on Designing Data-Intensive Applications

## Chapter 1: SLOs, SLAs and stats

- The best way to see if users experienced any delays (for example) is measuring via percentiles of response times, then seeing them from fastest to lowest - then the meadin is the halfway point: if the median is 300ms, then half you users got responses faster than that and the other half got them slower than that

- Also, how bad are your outliers? The customers with the most data usually make you the most money so take care of them - if your outlier users are on p95, p99 and p999 and your 95th percentile response time is 1.5 seconds, then 95% of requests take lower than that, however, 5% of them take more than 1 and a half SECONDS. For your most lucrative users.

- Thus, percentiles are used in Service Level Objectives (SLOs) and Service Level Agreements (SLAs) - These contracts define expected performance, so, a service that has a SLA of median response time of less than 200ms and a p99 under 1 second and suddenly it doesn't, it might as well be down.

- "These metricts set expectations for clients of the service and allow customers to demand a refund if the SLA is not met"

## Chapter 2: Databases

- MySQL copies the entire table on ALTER TABLE instead of in a few miliseconds, like PostgreSQL and SQLServer.

### Document DBs

- "The structure of the data is determined by the external systems over which you have no control and which may change at any time" - this means integration APIs are good candidates for NoSQL databases like MongoDB.

- On updates to a document, the entire document needs to be rewritten

- "MapReduce is a programming model for processing large amounts of data in bulk accross many machines" - page 46

- map and reduce are pretty restricted as they must be pure functions only. Thus, they only use data passed to them as input, cannot have any other queries and can't have side-effects.

### Graph DBs

- Example of graph-structured data - page 50

