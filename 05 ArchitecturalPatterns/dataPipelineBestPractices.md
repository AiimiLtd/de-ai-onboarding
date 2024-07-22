# **Data Pipeline Best Practices**
Generally, following these data pipeline best practices will result in a data pipeline that exhibits the following characteristics:

- Efficiency
- Scalability
- Reliability
- Resilience
- Security

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