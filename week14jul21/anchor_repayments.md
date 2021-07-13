## 18. Anchor Repayments

The below dashboard shows the current state of the Anchor Protocol with **current outstanding debt at 250M UST.** The **current collateral deposits stand at close to 1 Billion UST with a LTV ratio of 25%.** The dashboard also visualises daily borrows and repayments. During the period between May 19th to 23rd the repayments surpassed borrows due to crash in LUNA prices.   

### Key Metrics  
* Deposits 🔺 in both bLUNA & USD with total deposits close to 1 billion USD 🔥
* Uptrend in amount borrowed ⬆️
* LTV ratio is at a healthy 25.5 % ❇️
* So far 15K borrowers have borrowed close to 1.1 billion USD 🔥
* 2X increase in addresses interacting with the protocol

<iframe width="1024" height="612" src="https://app.powerbi.com/view?r=eyJrIjoiNGFhMDcxNjUtYWZhMC00MDcwLWI3NjYtMjg2YWUzMDZmZTk1IiwidCI6ImIyNzI1YWM4LTMyY2MtNDhjZS1iYTdmLTc4MmFlYjQxNTUwYSJ9" frameborder="0" allowFullScreen="true"></iframe>


### LTV

LTV ratio determines whether the protocol is sufficiently collateralized at all times to avoid default. The LTV is calculated as the ratio of value of deposits to the amount borrowed. The protocol has maintained a healthy ratio of around 30% till the May 19th price crash where the LTV ratio drops to 14%. As the price volatility has settled you can see the LTV ratio increasing showing a marked increase in borrowers risk appetite.  

You can also see a marked 2X increase in addresses interacting with Anchor protocol.  

<iframe width="1024" height="612" src="https://app.powerbi.com/view?r=eyJrIjoiMjhjYWFiOTktMDBlYi00NDA5LWE4NTEtNmM2ZGE1ZDViMGE4IiwidCI6ImIyNzI1YWM4LTMyY2MtNDhjZS1iYTdmLTc4MmFlYjQxNTUwYSJ9" frameborder="0" allowFullScreen="true"></iframe>

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
