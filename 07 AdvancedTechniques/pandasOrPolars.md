# Pandas or Polars?

Pandas and Polars are both popular data manipulation libraries in Python. Here's a quick breakdown of their pros and cons:

## Pandas

|Pros|Cons|
|---|---|
|Mature and widely used library with extensive documentation and community support.|Can be memory-intensive for large datasets due to its reliance on NumPy arrays.|
|Offers a wide range of data manipulation and analysis functionalities.|Performance is usually slower compared to Polars.|
| Pandas integrates well with other popular Python libraries such as NumPy, Matplotlib, and SciPy, enhancing its capabilities for data analysis and visualization.|Limited support for parallel processing and distributed computing. (it normally runs on a single thread)|

## Polars

|Pros|Cons|
|---|---|
|Designed for high-performance data manipulation and analysis.|Relatively new library with a smaller community compared to Pandas.|
|Utilizes Rust under the hood, resulting in faster execution times.|Resources may be more limited.|
|Supports lazy evaluation, enabling efficient handling of large datasets.|Some advanced features available in Pandas may not be fully implemented in Polars yet.|
|Provides a convenient API similar to Pandas, making it easy to transition between the two.||
|Offers parallel processing capabilities out of the box, for improved performance.||

### Polars Technical Details

Polars being built in rust ensures that it doesnt use any other python libraries like pandas uses numpy, this way it can then utilise multiple threads and cores to process data, something that pandas cannot do.  
Rust also uses the Arrow memory format, this is also used by other big data tools like Apache Spark, Parquet files and the Cassandra database.  
Polars also has a lazy evaluation engine, this means that it can optimise the execution of the code, this is similar to how Spark or SQL works. In contrast pandas will execute the code line by line, with no optimisations.

## Conclusion

While Polars is the less mature option, I encourage data engineers to explore its capabilities, especially when dealing with large datasets or performance-critical applications.
