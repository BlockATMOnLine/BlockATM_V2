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

# 付币合约

付币合约用于批量付币，由合约中指定的"授权签名地址"签名发起付币申请，由 BlockATM 付币代理合约执行付币操作。

{% hint style="info" %}
创建付币合约时，需指定"授权签名地址"，一旦指定并成功创建合约后，无法修改，以此保证合约资产的安全性。
{% endhint %}

### 合约权限说明

<table><thead><tr><th width="179.046875">地址类型</th><th>说明</th><th>权限</th></tr></thead><tbody><tr><td>管理员地址</td><td>创建收币合约的钱包地址</td><td>管理付币合约</td></tr><tr><td>授权签名地址</td><td>有权限提取收币合约资产的钱包地址</td><td>付币</td></tr></tbody></table>

### 付币智能合约代码

{% tabs %}
{% tab title="构造函数" %}
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

{% tab title="付币逻辑" %}
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

### 历史合约版本

### V2

2025 年 4 月 17 日

* 升级到 Web3 自托管框架付币合约
* 提供基于 API 自动化和 Excel 手动两种上传付币订单方式

### V1

2023 年 10 月 22 日

* 基于付币客户端实现自助提币
* 基于 Web3 SDK 进行合约自助交互





