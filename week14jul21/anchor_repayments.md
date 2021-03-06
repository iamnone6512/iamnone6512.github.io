## 18. Anchor Repayments

The Anchor protocol defines a money market between a lender, looking to earn stable yields on their stablecoins, and a borrower, looking to borrow stablecoins on stakeable assets. To borrow stablecoins, the borrower locks up Bonded Assets (bAssets) as collateral , and borrows stablecoins below the protocol-defined LTV ratio. Borrowers can currently use bLuna as collateral to deposit and borrow stable coins up to a maximum LTV of 50%.

The below dashboard shows the current state of Anchor Protocol. Borrowers have deposited 78 million bLUNA as collateral with a TVL close to 600 million USD.Both the amount borrowed and the protocol users are on the rise.    

###### Data refreshed till 9th July 2021
<iframe width="1024" height="612" src="https://app.powerbi.com/view?r=eyJrIjoiOWRmYjU3YjgtNTU2Ny00YmU1LWE1M2EtMTcyZDU1NWYzMzQ0IiwidCI6ImIyNzI1YWM4LTMyY2MtNDhjZS1iYTdmLTc4MmFlYjQxNTUwYSJ9" frameborder="0" allowFullScreen="true"></iframe>


### LTV

The LTV ratio assesses whether the protocol is adequately collateralized to avoid default at all times. The LTV is computed as the ratio of the deposit value to the loan amount. As the price volatility has subsided, the LTV ratio has risen, indicating a significant increase in the risk appetite of the borrowers.

In addition, the number of addresses engaging with the Anchor protocol has increased in the last 30 days. 
###### Data refreshed till 9th July 2021
<iframe width="1024" height="612" src="https://app.powerbi.com/view?r=eyJrIjoiMThlZDZkNjEtYmJhMi00ODI3LTk2ZDktMzBmYjkxNGE1ZDhhIiwidCI6ImIyNzI1YWM4LTMyY2MtNDhjZS1iYTdmLTc4MmFlYjQxNTUwYSJ9" frameborder="0" allowFullScreen="true"></iframe>

#### Developer Notes  

A lending protocol has four major steps in its lifecycle,

* Deposit Collateral
* Borrow Amount
* Repay Amount (Partial or Full)
* Withdraw Collateral (Partial or Full)

We look in detail how we have decoded the transactions to arrive at these numbers,
  
##### Deposit & Withdraw Collaterals  

The deposit & withdraw collateral has two sets of events in the transaction, the one which deposits/ withdraws the collateral and the other one locks/ unlocks the collateral in the Anchor Overseer Contract. We look for the lock/ unlock collateral message to get the amount deposited. We join with terra.oracle_prices to get prices in USD.

> msg_value : execute_msg = 'lock_collateral'  ||  msg_value : execute_msg = 'unlock_collateral'


##### Borrow & Repay Amounts
  
Calculating borrow and repay amounts is straightforward as we look for the following messages 

> msg_value : execute_msg = 'borrow_stable' || msg_value : execute_msg = 'repay_stable'  

#### Links
###### Repayments Query : <https://app.flipsidecrypto.com/velocity/queries/7fb417fa-1226-4fcd-9ba7-4cf3350dc5a3>
###### LTV Query: <https://app.flipsidecrypto.com/velocity/queries/8c3921fa-0422-4bcf-bbaf-e9a3e9ebb1c5>
######  Sample Transactions : 
###### Deposit : <https://finder.terra.money/columbus-4/tx/1520E5FC78430874E7CBD35617421590C2E9364752AF9FB98A64B479076A204F>
###### Borrow : <https://finder.terra.money/columbus-4/tx/908DB612BB3222E4A3F05257D8EB45605EB45E48DA7035FC7281C4620B84E585>
###### Repay : <https://finder.terra.money/columbus-4/tx/206D4BA99EC4B49231BD09C1F1E181FFC7FF79DD03FB09C6652E94E6928A3F90>
###### Withdraw : <https://finder.terra.money/columbus-4/tx/292F28C5E1B543C75391703B75970E51C9049D7A5645A964884A5770E3BD618E>
