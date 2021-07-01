## Hello


```sql
SELECT block_timestamp :: DATE,
       Avg(price_usd) - 1 AS peg_variance
FROM   terra.oracle_prices
WHERE  symbol = 'UST'
       AND block_timestamp :: DATE >= '2021-01-01'
GROUP  BY 1 
```
