- Ref: https://www.youtube.com/watch?v=zrl_odkY5tI
- `DATE` : Stores date but not time. Range: (1000-01-01 to 9999-12-31 23:59:59)
- `DATETIME` : Store date and time. Range: (1000-01-01 00:00:00 to 9999-12-31 23:59:59)
- `TIMESTAMP` : Stores date and time (Unix epoch as integer). Range: (1980-01-01 00:00:00 UTC to 2038-01-19 03:14:07 UTC). It has limit of 0 to 2^32 seconds. Database stores it as integer however when reading the drivers generally convert it to `DATETIME` 
- To store at microseconds level granularity database provides fractional seconds as optional which ranges 0 to 3 bytes depending on precision.
- 1, 2 precision: 1 byte

## DATETIME:
1. When you want to store usecase specific time. e.g: Appointment, Schedules etc.
2. When you want to do calculations within MySQL e.g `INTERVAL 1 DAY` etc.
3. Stored as compact 5 byte representation, covers a longer range than timestamp.
4. Human readable and native language object support.
5. Better to use if data is timezone sensitive and user specific. However need to beware of the region case as the insert value will not be generic and always region specific. 
   
## TIMESTAMP:
1. Stored as integers representing time in UTC.
2. Driver will do conversion of timezone as it'll always be stored in UTC by database.
3. TIMESTAMP is lightweight than DATETIME as it's just 4 bytes while other is 5 bytes.
4. Use timestamps when you want to record system time. e.g `created_at, metric_ingest_time` etc.