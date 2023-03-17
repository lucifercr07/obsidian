- Question: Hacker rank [link](https://www.hackerrank.com/challenges/weather-observation-station-5/problem?isFullScreen=true&h_r=next-challenge&h_v=zen&h_r=next-challenge&h_v=zen&h_r=next-challenge&h_v=zen&h_r=next-challenge&h_v=zen)
- https://stackoverflow.com/questions/12062565/use-a-second-order-by-in-mysql-query
- https://stackoverflow.com/a/514951/4134429
```sql
ORDER BY article_rating, article_time DESC
```

will sort by article_time only if there are two articles with the same rating. From all I can see in your example, this is exactly what happens.

```sql
↓ primary sort                         secondary sort ↓
1.  50 | This article rocks          | Feb 4, 2009    3.
2.  35 | This article is pretty good | Feb 1, 2009    2.
3.  5  | This Article isn't so hot   | Jan 25, 2009   1.
```

but consider:

```sql
↓ primary sort                         secondary sort ↓
1.  50 | This article rocks          | Feb 2, 2009    3.
1.  50 | This article rocks, too     | Feb 4, 2009    4.
2.  35 | This article is pretty good | Feb 1, 2009    2.
3.  5  | This Article isn't so hot   | Jan 25, 2009   1.
```
