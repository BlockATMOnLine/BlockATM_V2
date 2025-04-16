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

# 收币智能合约

BlockATM 的收币智能合约有两类：支付智能合约、转账智能合约

### 支付智能合约

支付智能合约提供安全可靠的收币支付方式，用户通过连接钱包并授权智能合约自动执行交易完成支付，用户支付的加密货币托管在收币智能合约中，若要提取收币智能合约中的资产，需合约中指定的财务钱包地址发起提币，并且只能提到合约中指定的接收钱包地址。

{% hint style="info" %}
创建收币智能合约时，需指定财务钱包地址和提币接收钱包地址，一旦指定并成功创建合约后，无法修改，以此保证合约资产的安全性。
{% endhint %}

### 转账智能合约

转账智能合约提供方便快捷的收币支付方式，用户通过手机钱包扫描接收地址的二维码进行转账支付，用户支付的加密货币将托管在接收地址中，若要提取接收地址中的资产，需合约中指定的财务钱包地址发起提币，并且智能提到合约中指定关联的收币智能合约，再从收币智能合约中提取资产。

{% hint style="info" %}
创建转账智能合约时，需指定财务钱包地址和关联的收币智能合约地址，即创建转账智能合约的前提是先创建收币智能合约。
{% endhint %}

### 合约权限说明

<table><thead><tr><th width="179.046875">钱包类型</th><th>说明</th><th>权限</th></tr></thead><tbody><tr><td>管理员钱包</td><td>创建收币智能合约的钱包为管理员</td><td>删除收币智能合约</td></tr><tr><td>财务钱包</td><td>有权限提取收币智能合约资产的钱包</td><td>提币</td></tr><tr><td>提币接收钱包</td><td>唯一能接收收币智能合约资产的钱包</td><td>接收财务提币资产</td></tr></tbody></table>

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

* 解藕收币智能合约与收银台：独立收银台主体，由企业自主决定收银台关联的收币智能合约
* 更改收币手续费收取规则：由押金中收取改为从收币合约余额中收取

### V1

2023 年 7 月 10 日

* 收币智能合约指定了财务钱包和提币接收钱包，只有财务有权限提取合约资产到提币接收钱包；并且合约一经创建则不可篡改
