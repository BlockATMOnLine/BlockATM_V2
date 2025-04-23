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

# 收币合约

BlockATM 收币合约分为两类：Web3收币合约、扫码收币合约

### Web3收币合约

Web3收币合约提供安全可靠的收币支付方式，用户通过连接钱包并授权智能合约自动执行交易完成支付，用户支付的加密货币直接转入Web3收币合约，有且只有合约中指定的"授权签名地址"有权限提币，并且只能提到合约中指定的"资产接收地址"。

{% hint style="info" %}
创建收币合约时，需指定授权签名地址和资产接收地址，一旦指定并成功创建合约后，无法修改，以此保证合约资产的安全性。
{% endhint %}

### 扫码收币合约

扫码收币合约提供方便快捷的收币支付方式，用户通过手机钱包扫描接收地址的二维码进行转账支付，有且只有合约中指定的"授权签名地址"有权限提币，并且只能提到合约中指定的"资产接收地址"。

{% hint style="info" %}
创建扫码收币合约时，需指定授权签名地址和资产接收地址，但扫码收币合约的资产接收地址必须是Web3收币合约地址，即创建扫码收币合约的前提是先创建Web3收币合约。
{% endhint %}

### 合约权限说明

<table><thead><tr><th width="153.0390625">地址类型</th><th width="349.3359375">说明</th><th>权限</th></tr></thead><tbody><tr><td>管理员地址</td><td>创建收币合约的钱包地址</td><td>管理收币合约</td></tr><tr><td>授权签名地址</td><td>有权限提取收币合约资产的钱包地址</td><td>提取合约资产</td></tr><tr><td>资产接收地址</td><td>有权限接收收币合约资产的钱包地址</td><td>接收合约资产</td></tr></tbody></table>

### 收币智能合约代码

{% tabs %}
{% tab title="构建函数" %}
```solidity
/**
函数：constructor
功能：商户在部署收款合约时，在合约部署时设定提现地址、财务人员地址、管理员等
@param newWithdrawList 提现地址列表，在合约部署时写入后不可修改
@param newFinanceList 财务人员地址列表，在合约部署时写入后不可修改
@notice 该构造函数用于初始化合约的关键参数和权限设置。
@notice 提现地址列表和财务地址列表不能为空，确保合约初始化时具备必要的权限配置。
@notice 手续费网关地址用于处理与费用相关的逻辑，确保系统收益和费用扣除的透明性。
*/
constructor(
bool safe,
uint256 id,
address[] memory newWithdrawList,
address[] memory newFinanceList,
address newFeeGateway
) {
//参数安全性检查
...
// 设置提币地址
processList(newWithdrawList, withdrawMap);
withdrawList = newWithdrawList;
// 设置财务地址
processList(newFinanceList, financeMap);
financeList = newFinanceList;
 //其他参数设置
....
// 设置合约owner
owner = msg.sender;
}
```
{% endtab %}

{% tab title="付币方法" %}
```solidity
/*
 * 函数：deposit
 * 功能：用户将代币支付到收款合约中，并记录交易。
 * @notice 函数会记录充值次数，用来计算手续费总额
 * @notice 函数会触发 `Deposit` 事件，记录详情。
 */
function deposit(
    address tokenAddress,
    uint256 amount,
    string calldata orderNo
) 
    public 
    checkTokenAddress(tokenAddress) 
    returns (bool) 
{
    //  必须的参数和状态检测
    ... 

    // 调用ERC20代币转账
    uint256 finalAmount = super.transferCommon(tokenAddress, address(this), amount);

    // 更新该代币地址的充值次数，用于计算手续费
    transferMap[tokenAddress] += 1;

    // 触发 Deposit 事件，记录充值详情
    emit Deposit(msg.sender, address(this), tokenAddress, finalAmount, orderNo);

    return true;
}
```
{% endtab %}

{% tab title="提币方法" %}
```solidity
/**
 * 函数： withdraw
 * 功能： 商户批量提取合约中资产
 * 限定：onlyFinance 该函数仅允许财务角色调用，确保只有授权人员可以执行提现操作。
 * @param recipientAddress 提币地址必须为商户提币固定地址，无法更改，防止资金被错误发送到其他地址。
 * @param withdrawRequests 提现请求数组，支持多种代币批量同时提取。
 * @notice 每次提现会计算手续费，并将手续费发送到指定的手续费地址。
 * @notice 触发 `Withdraw` 事件，记录提现详情，便于审计和追踪。
 */
function withdraw(
    bool isSafeTransfer,
    Withdraw[] calldata withdrawRequests,
    address recipientAddress
) 
    public 
    onlyFinance 
    returns (bool) 
{
    // 限定提币地址只能为设定的固定地址 
    require(recipientAddress == FIXED_WITHDRAW_ADDRESS, "提币地址必须为固定地址，无法更改");

    // 其他提币参数安全检查
    ...

    // 遍历循环处理提币请求
    for (uint256 i = 0; i < requestCount;) {
        
        //提币金额和手续费计算 
        ...
        //执行提币转账
        super.withdrawCommon(isSafeTransfer, request.tokenAddress, recipientAddress, userAmount);
        
        //执行手续费转账
        super.withdrawCommon(isSafeTransfer, request.tokenAddress, feeCollector, fee);
    }

    // 触发提现事件，用于BlockATM事件监听
    emit Withdraw(msg.sender, recipientAddress, withdrawRequests, feeAmounts, feeCollector);

    // 返回 true 表示提现成功
    return true;
}
```
{% endtab %}
{% endtabs %}

***

### 历史合约版本

### V2

2025 年 4 月 17 日

* 独立收银台主体，解藕收币合约与收银台，实现由企业自主决定收银台关联的收币合约
* 更改收币手续费规则：从押金抵扣方式改为从收币合约余额中收取，降低企业展业成本

### V1

2023 年 7 月 10 日

* 收币合约指定了授权签名地址和资产接收地址，只有授权签名地址有权限提取合约资产到资产接收地址
