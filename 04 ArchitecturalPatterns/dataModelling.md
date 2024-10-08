# **Approaches to Data Modelling**

Data modelling is an art, not a science. Data modelling is the process of structuring data to support efficient storage, retrieval, and use. The success of any data model hinges on an **understanding of the intended use cases** and the skills of the end users. All choices have trade-offs.  

As we are consultants, we need to approach any problem with the customer in mind - they will inherit the build and therefore need to be comfortable with the solution.

The headline is: *work backwards*. Let's explain what that means...

## **Work backwards: Build understanding**

Your first areas of focus should be:

* **Data access patterns**:  
  * How do you anticipate end-users will interact with the data? Are they always retrieving single records? Or do they need access to large swathes of data, e.g. grouped by a key variable? Do they always filter by certain fields? How will the data literally be accessed? (BI tools, CLoud platforms such as Databricks, Snowflake etc)
* **Business goals & aspirations**:
  * The model you build must be suitable for current use cases, but also take the time to understand the future aspirations of the client. Do they anticipate scaling rapidly? Do they want to introduce streaming data?
* **End-user skills**:
  * Who are your end-users? Are you designing a model which will be used by skilled data analysts? Or are you designing a model which will be served directly to external customers?
* **The broader architecture of the solution**:
  * Where will the data be surfaced? Will any subsequent processes consume the data?
* **Data types**
  * Is the data in question structured, semi-structured, unstructured, or a combination?

Once you understand these issues, you can make a more well informed decision on what to build.

## **Approaches to Relational Modelling**

### **Kimball**

Kimball is a dimensional modelling approach. It is designed to make data more accessible to end-users, and is particularly suited to data warehousing and business intelligence applications. The Kimball approach is based on the idea of a star schema, where a central fact table is surrounded by dimension tables. This makes it easy to query the data and generate reports.

Proper understanding of the Kimball approach is essential for any data engineer working in the field of business intelligence, however we should not be scared to deviate from this approach if the use case demands it. No one approach is perfect for every situation.

### **One Big Table (OBT)**

The One Big Table approach is the opposite of the Kimball approach. Instead of breaking the data down into smaller tables, the One Big Table approach combines all the data into a single table. This can make it easier to query the data, but can also make it harder to manage and maintain.

The One Big Table approach is not as widely used as the Kimball approach, but it can be useful in certain situations. For example, if you have a small dataset that is not going to change very often, the One Big Table approach might be a good choice.

One Big Table is also a good choice if your end-users are not very skilled in data analysis, as it can make the data more accessible to them.

## **Inmon**

Less bothered about this, but still to do...

## **Approaches to NoSQL Modelling**

Focussing on Azure CosmosDB (Horiz. scaling, partitioning (PK), access patterns etc)

To do

Flesh out my "block design" for NoSQL...

```JSON
{
  "id": "123-456-789",
  "discriminator": "guide",
  "details": {
    "title": "Approaches to Data Modelling",
    "topic": "NoSQL",
    "skillRequired": 10,
  },
  "documentMetadata: {
    "createdOn": "...",
    "createdById": "..."
    ...
  }
}
```
