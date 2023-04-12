- Ref: https://www.youtube.com/watch?v=Ofk7G3GD9jk and https://www.youtube.com/watch?v=pZQsDloGB4w
### RDD:
1. Resilient Distributed Dataset
2. Logical distributed data abstraction distributed across clusters, divide in partitions.
3. On these distributed dataset we can write lambda, operations etc.
4. Resilient: Ability to recreate RDD at any point of time from where it began. Recorded as lineage of where we came from(original RDD)
5. Immutable
6. Compile-time Type-Safe: Integer RDD, String or Text RDD, Double or Binary RDD
7. Unstructured/Structured Data: Text (Logs, tweets, articles etc.)
8. Lazy: Don't get materialized until an action is performed. Transformations just build the acyclic graph chain, until action is performed it won't materialize.
   ![[Pasted image 20230322174506.png]]
9. To use when:
   -  Low-level API & control of dataset
   - Don't care schema or structure of data
   - Dealing with unstructured data
   - Manipulate data with lambda functions than DSL
   - Sacrifice optimization, performance and inefficiencies
10. Disadvantages:
    - Not optimized by spark
    - Slow for non-JVM language like Python
    - Inadverdent inefficiencies

### What is in an RDD?
- Dependencies: Based on lineage of transformations which are interdependent
- Partitions (with optional locality info)
- Compute function(`Partition => Iterator[T]`): Given a particular partition returns an iterator which will excute the particular code on that partition distributed across cluster and merges it back.
  The problem with this is the function is opaque to spark thus it doesn't optimize it.

### Structured APs In Spark:
![[Pasted image 20230322184518.png]]
- DataFrame is an alias of Dataset (Scala). DataFrame = Dataset[Row]
- In Java there is no DataFrame only Dataset as everything is Typed or enforces data types.
- Dataframe API's are untyped API's since the type will only be known during the runtime. Whereas dataset API's are typed API's for which the type will be known during the compile time.
- Dataframe loses type safety and can cause different type of reference casting errors.
  ![[Pasted image 20230325215746.png]]
- Datasets are extenstion to Dataframes and are type-safe. **{Try to use Dataset as much as possible}**
- ![[Pasted image 20230322185620.png]]
- Datesets take less memory than RDDs.
- Datasets faster for serialization/deserialisations.
- **Why to use Dataframes and Datasets:**
   - High level APIs and DSL
   - Strong type safety
   - Ease of use and readability
   - Code optimization and performance
   - Space efficiency with Tungsten (codename for the umbrella project to make changes to Apache _Spark's_ execution engine)
![[Pasted image 20230322190349.png]]

### Datasets
- Like and RDD, Dataset has a type
  ```scala
  // Read dataframe from a JSON file
  val df = sqlContext.read.json("people.json")
  // Convert the data to a domain object
  case class Person(name: String, age: Long)
  val ds: Dataset[Person] = df.as[Person]
```
- Datasets tend to use less memory as spark understands the datastructure of the datasets and it generates the encoders which allows it to transform it to a compact storage format and do operations on it and at end of it convert it back to the type like `Person` in above case.
- Memory is conserved because of the compact format. Speed is improved by custom code-generation. The penalty of serializing and deserialising goes away.
-  Datasets are more memory efficient compared to RDDs.
- Spark has to serialise the data a lot and is moved across clusters on network. Serialised data in Tungsten format which is done for Dataset will ofetn be 2x smaller in size which in turn reduces the disk and network usage.