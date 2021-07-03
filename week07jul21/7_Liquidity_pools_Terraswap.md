## 7. [Hard] Liquidity Pools on TerraSwap

 
###### Please Note: This is an interactive dashboard 

<iframe width="1024" height="612" src="https://app.powerbi.com/view?r=eyJrIjoiZjIwYjM5MGItZDk5Zi00ODNkLTg5MDAtYzI4Mzk3NzZhZDNiIiwidCI6ImIyNzI1YWM4LTMyY2MtNDhjZS1iYTdmLTc4MmFlYjQxNTUwYSJ9&pageName=ReportSection" frameborder="0" allowFullScreen="true"></iframe>


#### Developer Notes



##### SQL 
```sql
SELECT 
  BLOCK_DATE, 
  pair, 
  count(distinct transaction_id) as txn_count, 
  count(distinct wallet_address) as user_cnt 
from 
  (
    SELECT 
      msgs.block_timestamp :: DATE as BLOCK_DATE, 
      msgs.block_timestamp as block_timestamp, 
      msgs.tx_id as transaction_id, 
      msgs.msg_value : sender as wallet_address, 
      labels.address as contract_address, 
      labels.address_name as pair, 
      msg_events.inp_amount, 
      msg_events.inp_currency, 
      msg_events.fee_amount, 
      msg_events.fee_currency, 
      msg_events.tgt_amount, 
      msg_events.tgt_currency 
    from 
      (
        SELECT 
          * 
        from 
          terra.msgs msgs, 
          lateral flatten(input => msg_value : execute_msg) fl
      ) msgs 
      inner JOIN terra.labels labels on msgs.msg_value : contract = labels.address 
      AND upper(label)= 'TERRASWAP' 
      and UPPER(label_subtype) = 'POOL' 
      AND upper(msgs.key)= 'SWAP' 
      AND UPPER(msgs.tx_status)= 'SUCCEEDED' 
      INNER JOIN (
        SELECT 
          tx_id, 
          src.value : amount as inp_amount, 
          src.value : denom as inp_currency, 
          fee.value : amount as fee_amount, 
          fee.value : denom as fee_currency, 
          tgt.value : amount as tgt_amount, 
          tgt.value : denom as tgt_currency 
        from 
          terra.msg_events, 
          lateral flatten(
            input => event_attributes : "0_amount"
          ) src, 
          lateral flatten(
            input => event_attributes : "1_amount"
          ) fee, 
          lateral flatten(
            input => event_attributes : "2_amount"
          ) tgt 
        where 
          upper(event_type)= 'TRANSFER'
      ) msg_events ON msgs.tx_id = msg_events.tx_id 
    WHERE 
      block_date BETWEEN '2021-04-01' 
      AND '2021-06-30'
  ) A 
group by 
  1, 
  2 
order by 
  1, 
  2

```

##### Useful Links
###### Link to Query : <https://app.flipsidecrypto.com/velocity/queries/7a5099db-f7be-4d74-a511-77697077cdad>
###### Sample Transaction : <https://finder.terra.money/columbus-4/tx/4634FD40B67BBAFFC14EA64BB15A97C9B8B67AC8EF994957B782DAAFA558E501>
