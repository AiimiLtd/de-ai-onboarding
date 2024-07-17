# **NoSQL vs Relational**

## **Scenario**

A customer wants a Database to power their customer-facing web application. They have tens of thousands of users per day. The website holds product information, and the frequent access patterns are:

* single item reads
* single item writes

Reporting is also important. They'd like Dashboard, updated daily, for their morning management meetings. The data does not need row-level security etc.

## **Analysis**

**Observations**:

* The application is web-based, therefore low latency is needed to enhance user experience.
* The main access patterns are single item based, which lends itself to document-based databases.
* Reporting is needed which lends itself to relational databases (or similar)
* Reports will not be frequently used
* Reports are not subject to row-based security constraints

You'll not that there are competing realisations - this is okay.

**Suggested Approach**

One approach could be to run a Transactional focussed database to power the Website, acheiving low latency and offering the chance to design NoSQL models and partitioning strategy tailored to the required access patterns, and then an Analytical database to power downstream activities.

**Technology Suggestions**

In Azure, we could utilise CosmosDB, a NoSQL database which optimises for scale and latency as the transactional store. For the analytical store, we could utilize Azure Synapse Link in order to get a columnar representation of CosmosDB for "free".

Next, we could extract data from CosmosDB using Synapse Link, and surface that data in a Serverless SQL Pool. Why serverless? The reporting requirements are only used once daily, meaning we don't need the overhead of a Dedicated SQL Pool. In addition, no row-level security features are needed.
