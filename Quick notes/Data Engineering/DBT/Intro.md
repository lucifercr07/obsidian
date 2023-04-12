- Ref: https://www.youtube.com/watch?v=M8oi7nSaWps
- dbt enabled engineers to transform data in their warehouses by simply writing `select` statements. dbt handles turning these select statements into tables and views.
- dbt does the `T` in ELT i.e. Transform processes. It's extremely good at transforming data already in data warehouse.
- Advantages:
  1. Anyone who knows SQL can author their own data pipelines
  2. Enables us to do version control, tests, documentation, re-usability
- Need to transformation of data:
  1. Cleaning/Standardisation
  2. Deduplication
  3. Restructuring for better consumption
  4. Filtering data
  5. Aggregation of data
  6. Joining datasets

### DBT
![[Pasted image 20230405235041.png]]
- Sits in b/w Raw data and Transformed data
- Acts as an orchestration layer at top of warehouse. Pushes down code to warehouse vs traditional tool that do it in memory for ETL.
- Core concepts:
  1. Express all transforms with SQL select
     - Express business logic in SQL.
     - Idempotence leading to repeatable builds
     - ![[Pasted image 20230405235540.png]]
2. Expressing relationship with `ref` statements
    - `{{ ref('MODEL_NAME') }}` handles automatically dependencies in dbt models
    - ![[Pasted image 20230405235900.png]]
3. Easily build tests to ensure model accuracy
    - Assumptions are written in `.yml` file
    - dbt can tests if a column: `Is unique, doesn't contain nulls, only contains certain values`
    - Can test anything that can be written as a query
    - ![[Pasted image 20230406000724.png]]
4. Documentation is accessible and easily updated
    - dbt generates documentation automatically based on descriptions from `.yml` files, model dependencies, model SQL, sources and tests, cloumn names and types and tables stats from information schema