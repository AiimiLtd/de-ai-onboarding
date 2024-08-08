# **Data Pipeline Best Practices**
While best practices provide a solid foundation for building robust and efficient data pipelines, it's important to recognise that each clients needs and constraints are unique. The specifics of the data sources, the nature of the data being processed, the regulatory landscape and the desired outcomes all play critical roles in shaping the design and implementation of your pipeline. 

Therefore, while best practices offer general guidelines, it's crucial to tailor them to your particular context. This might involve prioritising certain aspects over others, adapting techniques to fit your infrastructure, or innovating new solutions that better meet your specific challenges. Ultimately, a successful data pipeline is one that aligns closely with your clients goals and requirements, balancing best practices with practical considerations to achieve optimal performance and reliability.

With the above caveat in mind, here are some best practices to consider when implementing a data pipeline.

## **Understanding Your Source Systems**
During the design of a data pipeline, it is important to consider the limitations, capabilities and nuances of the source systems from which data is being extracted. Overburdening these systems can lead to performance degradation, angry source system owners and even outages. Here are some strategies and considerations to ensure that data extraction processes are efficient and non-disruptive:

* **System Performance and Capacity:** Before designing the data extraction process, evaluate the source systems performance characteristics and capacity limits. Interviewing the source systems technical team or owner can give a good insight into this.

* **Read-Replicas / Built-in Services:** Where possible, use features that are baked into the source system such as read-replicas and standby databases to offload data extraction from the source system. Use any built-in APIs or services provided by the source system, these are often optimised for data extraction. Using these pre-existing services reduces the load on the primary system and can improve the overall performance and reliability of the data extraction process. Again, having a relationship with the source system owner will ensure that you are aware of all the possible options.

* **Incremental Source System Loads:** Instead of extracting full datasets, use incremental data loading techniques. If the source system already has change tracking techniques baked in, those can be utilised if your pipeline. See the [Incremental Loads](#incremental-loads) section for more details.

* **Scheduled Loading Windows:** Schedule data extraction during off-peak hours or periods of low activity on the source system. This reduces the risk of contention and minimises the impact on the users of the system.

## **Choosing the Right Tool for the Job**
Selecting the appropriate tools and tech stack for your data pipeline is an important step. The choice of tools depends on various factors, including the type and volume of data, processing needs, the specific goals of the pipeline and the type of consumers at the pipelines destination. Here are some considerations for choosing the right tool for your data pipeline.

* **Data Characteristics:** Identify the types of data you will be handling (structured, unstructured, or semi-structured), their formats and their volume and velocity.
  
* **Processing Requirements:** Determine whether your pipeline needs to handle real-time data streams or batch processing. Some tools are optimised for real-time analytics, while others excel in batch processing. See the [Batch](04 ArchitecturalPatterns\etlPatterns.md#batch) and [Streaming](04 ArchitecturalPatterns\etlPatterns.md#streaming) sections for more details.

* **Scalability and Flexibility:** Consider the scalability needs of your pipeline. Choose tools that can scale horizontally to handle growing data volumes and are flexible enough to accommodate changes in data sources or processing requirements if the volume/velocity of data is expected to change over time.

* **Source / Destination Compatibility:** Ensure the tools you choose can integrate well with your data sources and destinations. For example, if your data sources are all cloud based, hosted in Microsoft Azure, data tools in the Azure tech stack (e.g. Azure Data Factory, Azure Synapse Analytics, Fabric) might be more appropriate.

* **Cost and Budget Considerations:** When choosing a tool, it is prudent to be aware of the potential costs. Typically, costing solutions is very challenging given the amount of variables that can have an impact on the cost. Using a cost/benefit approach that utilises relative costs helps in the decision making process.

* **Consider Your Consumers:** Knowing your audience is important when choosing the tools for the pipeline. For example, if the pipelines primary purpose is to facilitate making data available to report developers, choosing a tool aligns to the reporting teams existing programming skills (e.g a tool that allows querying data via SQL) will be beneficial and aid adoption.

## **Incremental Loads**
An incremental load is the selective movement of data from one system to another. An incremental load pattern will attempt to identify the data that was created or modified since the last time the load process ran. This differs from a full data load, which copies the entire set of data from a given source. The selectivity of the incremental design usually reduces the system overhead required for the loading portion of the data pipeline.

The selection of data to move is often based on time, typically when the data was created or most recently updated. In some cases, the new or changed data cannot be easily identified solely in the source, so it must be compared to the data already in the destination for the incremental data load to function correctly.

### Benefits of the Incremental Load 
* **Speed:** 
  They typically run considerably faster since they touch less data. Assuming no bottlenecks, the time to move and transform data is proportional to the amount of data being touched.

* **Reduced Risk:**
  Because they touch less data, the surface area of risk for any given load is reduced. Any load process has the potential of failing or otherwise behaving incorrectly and leaving the destination data in an inconsistent state.

* **Consistent Performance:**
  Incremental load performance is usually steady over time. If you run a full load, the time required to process is monotonically increasing because today’s load will always have more data than yesterday’s. Because incremental loads only move the delta, you can expect more consistent performance over time.

### When to use the Incremental Load
 The decision to use an incremental or full load should be made on a case-by-case basis. There are a lot of variables that can affect the speed, accuracy and reliability of the load process.

* The size of the data source is relatively large
* Querying the source data can be slow, due to the size of the data or the technical limitations
* There is a viable means through which changes can be detected
* Data is occasionally deleted from the source system, but you want to retain deleted data in the destination

### How to implement an Incremental Load
For loading just new and changed data, there are two broad approaches on how to handle this.

* **Source Change Detection:** 
  This is a pattern in which we use the selection criteria from the source system to retrieve only the new and changed data since the last time the load process was run. This method limits how much data is pulled into the data pipeline by only extracting the data that actually needs to be moved, leaving the unchanged data out of the incremental load cycle. Detecting changes in the source is the easiest way to handle delta detection, but it’s not perfect. Most of the methods for source-side change detection require that the source resides on a relational database, and in many cases, you’ll need some level of metadata control over that database. This may not be possible if the database is part of a vendor software package, or if it is external to your network entirely.

* **Destination Change Comparison:**
  If the source for a given data pipeline doesn’t support source change detection, you can fall back to a source-to-destination comparison to determine which data should be inserted or updated. This method of change detection requires a row-by-row analysis to differentiate unchanged data from that which has recently been created or modified. Because of this, you’ll not see the same level of performance as with source change detection. To make this work, all the data (or at least the entire period of data that you care about monitoring for changes) must be brought into the data pipeline for comparison. Although it doesn’t perform as well as source-side change detection, using this comparison method has the fewest technical assumptions. It can be used with almost any structured data source.

## **Idempotency**
Generally, an operation is termed idempotent if it can be applied multiple times without changing the result beyond its initial application. In terms of data pipelines, idempotence means that no matter how many times you execute the pipeline, the outcome should stay consistent after the first execution.

### **Idempotency in Data Pipelines**
Specifically, data pipelines should exhibit the following properties which contribute to them being considered idempotent.

* When re-executing a data pipeline, data should not be duplicated downstream.
* If the pipeline breaks or is down for a day, the next run should catch up.
* Whether running the pipeline today or historically, it should not result in, or cause unintended consequences.

### How to implement an Idempotent Pipeline
Generally speaking, to achieve an idempotent pipeline, every time data is curated and persisted to disk, the write process should honour the above principals. Following the below strategies will help with this.

* **Use a mechanism to identify each unique record:**
  * Use unique keys or natural keys to identify records. This will allow you to use upsert statements or other mechanisms to ensure that data is only inserted or updated if it doesn’t already exist.
  * If no such key is present in the source data, use a form of hashing to to create your own. Most database management systems come with hashing functions out of the box.
* **Replacing existing rows with new values from the source row (upserting using a MERGE statement):** 
  * Using a MERGE statement allows inserting/updating/deleting in a single atomic operation by using the unique record key.
  * For example, the below MERGE statement compares a source table to a target table based on a key column. If a target row is detected, it is updated. If a target row is not found, it is inserted.  

    ```SQL
    MERGE INTO target
    USING source
        ON source.key = target.key
    WHEN MATCHED THEN
        UPDATE SET *
    WHEN NOT MATCHED THEN
        INSERT *
    ```
  * MERGE syntax and functions vary by tech stack but generally the concept is the same. See the below link for the Databricks/Delta specific implementation.  
  https://learn.microsoft.com/en-us/azure/databricks/delta/merge

## **Error Handling**
Error handling is the practice of automating error responses/conditions to keep the pipeline running or alerting relevant individuals/teams. The following approaches detail methods for handling errors.

* **Retry Mechanisms:**
Pipelines often fail due to scenarios that are outside of our control such as network issues, hardware failures or other transient issues. Building in an automated retry mechanism will ensure that a pipeline can gracefully handle these scenarios. Be mindful to set a sensible maximum retry limit. 

* **Alerting:**
Alerts might come in the form of an email or direct message on a clients preferred messaging platform. When alerting, be mindful of alert fatigue. Alert fatigue refers to an overwhelming number of notifications which results in recipients ignoring them. Be sure to reserve alerts for scenarios that cannot be ignored.

* **Logging:**
Logging the executions of a pipeline as well as any error conditions will help with troubleshooting. Logging options depend on the tech stack being used and how the pipeline is orchestrated so there is no one-size-fits-all approach. However, it's important that an appropriate execution history is persisted in some way to understand the scope of a potential issue.