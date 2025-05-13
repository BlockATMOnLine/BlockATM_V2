---
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Payout Contract

The payout contract is used for bulk payouts, with the payout request initiated and signed by the "Authorized Signature Address" specified in the contract. The payout operation is then executed by the BlockATM payout proxy contract.

{% hint style="info" %}
When creating a payout contract, the "Authorized Signature Address" must be specified. Once specified and the contract is successfully created, it cannot be modified, ensuring the security of the contract’s assets.
{% endhint %}

### Contract Permissions Explanation

<table><thead><tr><th width="179.046875">Address Type</th><th>Explanation</th><th>Permission</th></tr></thead><tbody><tr><td>Owner</td><td>The wallet address used to create the payout contract.</td><td>Manage the payout contract.</td></tr><tr><td>Authorized Signature Address</td><td>The wallet address authorized to withdraw assets from the payout contract.</td><td>Payout</td></tr></tbody></table>

### Payout Smart Contract Code

{% tabs %}
{% tab title="Constructor Function" %}
```solidity
/**
* Function: payout constructor
* Purpose: Initializes merchant contract with critical parameters including finance addresses and proxy address during deployment.
* @param newFinanceList List of finance addresses for initializing financial permissions
* @param newProxyPayoutAddress Proxy address that has exclusive batch payout execution rights
**/
constructor(
    bool safe,
    uint256 id,
    address[] memory newFinanceList,
    address newProxyPayoutAddress,
    address newFeeGateway
) {
    // Parameter safety checks
    ...
    
    // Set proxy contract address
    proxyPayoutAddress = newProxyPayoutAddress;

    // Initialize finance addresses
    processList(newFinanceList, financeMap);
    financeList = newFinanceList;

    // Set contract owner
    owner = msg.sender;
    
    // Other initialization parameters
    ...
}
```
{% endtab %}

{% tab title="Payout Function" %}
```solidity
/**
* Function: payoutByContract
* Purpose: Handles batch payment operations
* Restriction: onlyFinancials(payoutAddress) Ensures only BlockATM-authorized financial addresses or proxy contracts can call this function
* @param orderNo Array of batch payment order numbers, uploaded by merchant finance via API or Excel
* @param array Array of recipient addresses, uploaded by merchant finance via API or Excel
* @param amount Array of payment amounts, uploaded by merchant finance via API or Excel
* @return bool Returns true indicating successful payment
**/
function payoutByContract(
    bool safe, 
    address tokenAddress, 
    uint256 total, 
    address payoutAddress, 
    string[] calldata orderNo, 
    address[] calldata array, 
    uint256[] calldata amount
) public onlyFinancials(payoutAddress) returns (bool) {
    // Calls internal payoutToken function to execute payment
    payoutToken(safe, payoutAddress, tokenAddress, total, 0, 1, orderNo, array, amount, 0);
    // Returns true indicating successful payment
    return true;
}

/**
* Function: payoutToken
* Purpose: Executes the payment process flow
*/
function payoutToken(
    bool safe, 
    address from, 
    address tokenAddress, 
    uint256 total, 
    uint256 gasAmount, 
    uint256 payType, 
    string[] calldata orderNo, 
    address[] calldata array, 
    uint256[] calldata amount, 
    uint256 id
) internal {
    // Parameter safety checks
    ...
   
    // Executes batch token transfers
    _transferTokens(safe, from, tokenAddress, total, gasAmount, payType, feeAmount);
    _processBatchPayments(safe, tokenAddress, array, amount, length);
    
    // Calculates and deducts processing fees
    super.withdrawCommon(safe, tokenAddress, IBlockFee(feeGateway).feeAddress(), feeAmount + gasAmount);
    
    // Emits payment completion event for BlockATM payment monitoring
    emit PayoutToken(from, tokenAddress, payType, feeAmount, gasAmount, orderNo, array, amount, msg.sender, id);
}
```
{% endtab %}
{% endtabs %}

***

### Historical Contract Versions

### V2

April 17, 2025

* Upgrade to the Web3 self-hosted framework payout contract.
* Provides two methods for uploading payout orders: automated upload via API and manual upload via Excel.

### V1

October 22, 2023

* Implement self-service withdrawals based on the payout client.
* Self-service contract interaction based on Web3 SDK.





