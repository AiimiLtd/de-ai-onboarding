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