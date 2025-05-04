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
* 函数：payout constructor
* 功能：商户在部署合约时初始化财务地址、代理地址等关键参数设置。
* @param newFinanceList 财务地址列表，用于初始化财务权限
* @param newProxyPayoutAddress 代理地址，限定批量执行代付权限
**/
constructor(
    bool safe,
    uint256 id,
    address[] memory newFinanceList,
    address newProxyPayoutAddress,
    address newFeeGateway
) {

    //参数安全性检查
    ...
    
    // 设置代理合约地址
    proxyPayoutAddress = newProxyPayoutAddress;

    // 设置财务地址
    processList(newFinanceList, financeMap);
    financeList = newFinanceList;

    // 设置合约所有者
    owner = msg.sender;
    
    //其他初始化参数设置 
    ...
}
```
{% endtab %}

{% tab title="Payout Logic" %}
```solidity
/**
* 函数：payoutByContract
* 功能：该合约用于处理批量付款操作
* 限定：onlyFinancials(payoutAddress) 确保只有BlockATM授权的财务地址或代理合约可以调用此函数
* @param orderNo  批量代付订单号数组，由商户财务通过api或excel上传
* @param array    批量接收地址数组，由商户财务通过api或excel上传
* @param amount   批量支付金额数组，由商户财务通过api或excel上传
* @return bool 返回 true 表示付款成功。
**/
function payoutByContract(bool safe, address tokenAddress, uint256 total, address payoutAddress, string[] calldata orderNo, address[] calldata array, uint256[] calldata amount) public onlyFinancials(payoutAddress) returns (bool) {
    // 调用内部函数 payoutToken 进行付款操作
    payoutToken(safe, payoutAddress, tokenAddress, total, 0, 1, orderNo, array, amount, 0);
    // 返回 true 表示支付成功
    return true;
}

// 函数：payoutToken
// 功能：执行付款的流程
function payoutToken(bool safe, address from, address tokenAddress, uint256 total, uint256 gasAmount, uint256 payType, string[] calldata orderNo, address[] calldata array, uint256[] calldata amount, uint256 id) internal {
    // 参数安全性检查
    ...
   
    // 执行代币批量转账操作
    _transferTokens(safe, from, tokenAddress, total, gasAmount, payType, feeAmount);
    _processBatchPayments(safe, tokenAddress, array, amount, length);
    
    // 执行手续费计算和扣除
    super.withdrawCommon(safe, tokenAddress, IBlockFee(feeGateway).feeAddress(), feeAmount + gasAmount);
    
    // 触发支付完成事件，用于BlockATM代付事件监听
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





