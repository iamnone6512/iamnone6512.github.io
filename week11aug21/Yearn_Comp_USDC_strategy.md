## 2. Returns on the ‘Generic Leverage Compound Farm’ strategy

The ‘Generic Leverage Compound Farm' is a USDC yVault strategy that accepts USDC deposits and transfers them to the Compound money market, where they generate interest. The approach is a leveraged one that utilises dYdX's flash loans. USDC deposits are accepted by the yVault, which is then allocated to the strategy . We'll look into how the strategy works and how yield is generated in detail.

The Compound money market provides yield in two different forms

* Users supply assets to the market, which are exchanged for cTokens for the equivalent amount. cTokens are yield bearing tokens, which means that the yield is included in the token exchange rate, and users can collect the yield for the time period held by simply exchanging the tokens.
* Compound governance token (COMP) is also provided as a reward for supplying and borrowing assets as an incentive.
* For borrowing assets the users have to pay interest but since they also get Compound governance token rewards for borrowing as well the overall APY is positive for borrowing as well. Hence users are getting paid to borrow as well from the money market.

So supplying and borrowing generates a positive yield the yearn strategy both supplies and borrows from compound to take on yield. The strategy also uses flash loans for leverage.

* A flash loan is a loan provided to the user and is only valid if it's paid back within the same block. The strategy uses dYdX as the provider for flash loans.

Now we look in detail how the strategy works, the base assets for this strategy USDC

1. Users deposit USDC into yVault which uses a collection of strategies to generate yield.
2. yVault transfers USDC to the ‘Generic Leverage Compound Farm’ for depositing into the  Compound USDC Money Market.
3. The strategy uses the USDC transferred from the vault and also adds a flash loan from dYdX and deposits both the amounts into the market.
4. cTokens are minted for the amount equivalent to the sum of yVault transfer and flash loans.
5. Next the amount equivalent to the amount loaned from the dYdX using flash loan is borrowed from Compound USDC market.
6. The flash loan is paid in full and closed.
7. Now the overall position of the strategy in the Compound USDC Money Market is, for supply its the sum of the transfers from yVault and the amount loaned using dYdX flash loan. For borrows it's the amount loaned using dYdX flash loan.
8. The strategy now earns interest and COMP rewards for the amount supplied and for the amount borrowed it pays interest but also earns COMP rewards. The APY rates are decided by the market but overall earns a positive APY due to COMP rewards. 
9. The farmed COMP rewards are swapped to USDC and deposited back making the strategy an auto compounding one.

The below dashboard shows the overall deposits in the strategy - 466 million USDC. It also shows the overall deposits split by deposit amount vs flash loans. You could see the leveraged flash loans exceed the user deposits but since it is a single asset strategy the chances of liquidation are very minimal.


.
<iframe width="1024" height="612" src="https://app.powerbi.com/view?r=eyJrIjoiZDUxMjZkYmItZDYyNC00YmUwLWI5ZDgtZGU3NGZhNGQwODVmIiwidCI6ImIyNzI1YWM4LTMyY2MtNDhjZS1iYTdmLTc4MmFlYjQxNTUwYSJ9" frameborder="0" allowFullScreen="true"></iframe>

Now we'll look at estimating the strategy's returns. We should compute the APY at the conclusion of each block, but we'll do it at the end of each day to keep things simple. Also  we don't use the cToken exchange rates for the calculation as the rates aren't linear when calculating the APY daily. 

For each day we get the total deposits, borrows and COMP reward tokens. We are calculating the  APY in the below dashboard as the following,

* total deposits * supply APY / 365 gets the Supply rewards
* total borrows * borrow APY / 365 gets the Borrow rewards
* For COMP rewards we take the amount swapped into USDC as its rewards
* total rewards = ( Supply rewards + COMP rewards ) - Borrow rewards
* APY is calculated as (total rewards / user deposits from yVault) * 365

As you can see the COMP governance token rewards make up for the bulk of the total rewards and the flash loans help to leverage the deposits, increasing the overall APY.


.

<iframe width="1024" height="612" src="https://app.powerbi.com/view?r=eyJrIjoiMmVlNTBkMDktMDk2Mi00ZWU4LTkwN2UtYzBjMDk2OTM5NTk2IiwidCI6ImIyNzI1YWM4LTMyY2MtNDhjZS1iYTdmLTc4MmFlYjQxNTUwYSJ9" frameborder="0" allowFullScreen="true"></iframe>


###### Link to Query : DEPOSITS/BORROWS <https://app.flipsidecrypto.com/velocity/queries/74bc95d3-e426-4282-8c3e-988c2952f779>
###### Link to Query : APY <https://app.flipsidecrypto.com/velocity/queries/a95f3831-fecd-4589-89e8-9944a9157089>
###### Decoded TX's[TX_Analysis.pdf](https://github.com/iamnone6512/iamnone6512.github.io/files/6965884/TX_Analysis.pdf)
