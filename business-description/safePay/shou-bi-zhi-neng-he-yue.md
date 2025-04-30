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

# Collection Contract

Collection contract is divided into two types: Web3 collection contract, Scan2pay contract.

### Web3 collection contract

The Web3 collection contract provides a secure and reliable way to collect coins. Users can connect their wallets and authorize smart contracts to automatically execute transactions for payment. The encrypted currency paid by users is directly transferred to the Web3 collection contract, and only the "authorized signature address" specified in the contract has permission to withdraw funds, which can only be withdrawn to the "asset receiving address" specified in the contract.

{% hint style="info" %}
When creating a web3 collection contract, you need to specify the authorized signature address and asset receiving address. Once specified and the contract is successfully created, it cannot be modified to ensure the security of the contract assets.
{% endhint %}

### Scan2pay contract

The scan2pay contract provides a convenient and fast way to collect coin payments. Users can transfer payments by scanning the QR code of the receiving address with their mobile wallet. Only the "authorized signature address" specified in the contract has permission to withdraw coins, and can only be withdrawn to the "asset receiving address" specified in the contract.

{% hint style="info" %}
When creating a scan2pay contract, you need to specify the authorized signature address and asset receiving address. However, the asset receiving address of the scan code receiving coin contract must be a Web3 collection contract address, which means that the premise of creating a scan2pay contract is to first create a Web3 collection contract.
{% endhint %}

### Contract permissions

<table><thead><tr><th width="250.23046875">Address type</th><th width="349.3359375">Introduction</th><th width="281.12109375">permissions</th></tr></thead><tbody><tr><td>Owner Address</td><td>Address for creating collection contract</td><td>Management Collection Contract</td></tr><tr><td>Authorized Signature Address</td><td>The address with permission to withdraw contract assets</td><td>Extract contract assets</td></tr><tr><td>Asset Receiving Address</td><td>The address that has permission to receive contract assets.</td><td>Receive contract assets</td></tr></tbody></table>

### Collection contract code

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

### Historical contract version

### V2

April 17, 2025

* The main body of the independent cashier, decoupling the collection contract from the cashier, allowing enterprises to independently decide on the collection contract associated with the cashier.
* Change the rules of receiving fees: change from deducting from the deposit to collecting from the balance of the receiving contract, reducing business expansion costs.

### V1

July 10, 2023

* The collection contract specifies the authorized signature address and asset receiving address, only the authorized signature address has permission to withdraw contract assets to the asset receiving address.
