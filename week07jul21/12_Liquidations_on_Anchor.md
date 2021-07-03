
## 12. [Hard] Liquidations on Anchor


<iframe width="1024" height="612" src="https://app.powerbi.com/view?r=eyJrIjoiN2Q1NmNiMmEtMzNlNy00NTc5LWExODUtYmM2OGU4MzcxZDcyIiwidCI6ImIyNzI1YWM4LTMyY2MtNDhjZS1iYTdmLTc4MmFlYjQxNTUwYSJ9" frameborder="0" allowFullScreen="true"></iframe>


```sql
SELECT b.block_timestamp::DATE as block_date,
  a.tx_id,
  trim(a.msg_value:execute_msg.liquidate_collateral.borrower,'"') as borrower, 
  trim(a.msg_value:sender,'"') as liquidator,
  b.liq_amount/1000000 as liq_amount,
  b.currency
  from terra.msgs a 
  inner join ( SELECT b.*,fl.value:amount as liq_amount,trim(fl.value:denom,'"') as currency 
  from terra.msgs b, lateral flatten(input => msg_value:coins) fl  WHERE
  b.msg_value:contract='terra1w9ky73v4g7v98zzdqpqgf3kjmusnx4d4mvnac6'   
  AND UPPER(b.tx_status)='SUCCEEDED' 
  AND   b.block_timestamp::DATE BETWEEN '2021-04-01' AND '2021-07-03') b
  ON 
  a.tx_id = b.tx_id
  AND a.msg_value:execute_msg.liquidate_collateral.borrower is not null
```

Link to Query : https://app.flipsidecrypto.com/velocity/queries/deb8a749-ae67-49b6-96b7-baf2df6fc391

Link to Sample Tansaction : https://finder.terra.money/columbus-4/tx/E53C5A191EF241B420B24CBEEFFF180FF2F54CD6163881D4739D1564245BE77B


https://docs.anchorprotocol.com/smart-contracts/liquidations/liquidation-contract


![Screen Shot 2021-07-02 at 5 17 09 PM](https://user-images.githubusercontent.com/86668287/124346339-d878c600-dbfb-11eb-99ee-25df2f7ca690.png)
