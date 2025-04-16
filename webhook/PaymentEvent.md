---
icon: plug-circle-plus
---

# Payment Event

当支付订单状态发生变更时，BlockATM将通过Webhook支付事件发送通知。包含以下状态类型：

PENDING（处理中）：当支付订单已达到要求的链上确认数

SUCCESS（支付成功）：当支付订单被确认已成功支付

EXPIRED（已过期）：因未完成支付导致客户订单超时

CANCELLED（已取消）：当客户主动取消支付订单

BlockATM将通过您配置的Webhook URL向服务器发送以下字段数据：

| name                      | comment                                                                                                                                                                                               | example                                    |
| ------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------ |
| id                        | The order number corresponding to the blockATM platform                                                                                                                                               | 8210000374                                 |
| orderNo                   | Optional. The order number corresponding to your platform.                                                                                                                                            | O20231022                                  |
| chainId                   | ChainId is a unique identifier used in blockchain networks to distinguish a particular network。.you can find the chainId in [https://chainlist.org/](https://chainlist.org/)                          | 1                                          |
| cashierId                 | <p>The payment cashier ID for receiving funds.</p><p><br></p>                                                                                                                                         | 199                                        |
| amount                    | Actual payment amount by the customer                                                                                                                                                                 | 13.410037                                  |
| custNo                    | Optional. Your customerNo who pay the order.                                                                                                                                                          | 860021                                     |
| network                   | Network name used to describe the occurrence of this transaction order. If you want to uniquely identify the corresponding network chain, it is recommended to use the chainId. (e.g: Ethereum, TRON) | Ethereum                                   |
| symbol                    | The token identifier that the customer pays with.                                                                                                                                                     | USDT                                       |
| txId                      | The transaction hash corresponding to the order on the blockchain. You can view the transaction details on the corresponding network's block explorer.                                                | 0x....                                     |
| orderType                 | 1：Smart Contract Payment 2.QR Code Payment 3:Smart Contract Address Direct                                                                                                                            | 1                                          |
| status                    | <p><strong>CANCELLED</strong><br>EXPIRED</p><p><strong>PENDING</strong></p><p><strong>SUCCESS</strong></p>                                                                                            | **SUCCESS**                                |
| <p></p><p>fromAddress</p> | <p>The address used to pay for this order (empty if unpaid).</p><p><br></p>                                                                                                                           | 0xa9e358E33a57E67c9B84618a52f0194C345C8e35 |
| <p></p><p>blockTime</p>   | The time the order was on the blockchain                                                                                                                                                              | 1693212861016                              |

| 字段名         | 说明                                                                                                                             | 示例值                                     |
|----------------|----------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------|
| id             | BlockATM平台对应的订单编号                                                                                                       | 8210000374                                 |
| orderNo        | (可选) 您平台对应的订单号                                                                                                        | O20231022                                  |
| chainId        | 区块链网络唯一标识符，可在[chainlist.org](https://chainlist.org/)查询                                                           | 1 (ETH主网)                                |
| cashierId      | 收款收银台ID                                                                                                                    | 199                                        |
| amount         | 客户实际支付金额                                                                                                                 | 13.410037                                  |
| custNo         | (可选) 支付订单的客户编号                                                                                                        | 860021                                     |
| network        | 交易发生的网络名称（建议使用chainId进行唯一标识）                                                                                | Ethereum                                   |
| symbol         | 客户支付的代币标识                                                                                                               | USDT                                       |
| txId           | 区块链上对应的交易哈希，可通过对应网络的区块浏览器查看详情                                                                       | 0x....                                     |
| orderType      | 订单类型：<br>1-智能合约支付<br>2-二维码支付<br>3-智能合约地址直连                                                               | 1                                          |
| status         | 订单状态：<br>**CANCELLED**(已取消)<br>**EXPIRED**(已过期)<br>**PENDING**(处理中)<br>**SUCCESS**(成功)                          | **SUCCESS**                                |
| fromAddress    | 支付地址（未支付时为空）                                                                                                         | 0xa9e358E33a57E67c9B84618a52f0194C345C8e35 |
| blockTime      | 订单上链时间戳                                                                                                                   | 1693212861016                              |
示例:



```json
{

​      "custNo": "CustNo_00240101",

​      "orderNo": "OrderNo_202504010023",

​      "id": 8210003616,

​      "symbol": "USDT",

​      "amount": "2000.00",

​      "status": "SUCCESS",

​      "txId": "0x7614a9840d9422feaef4671e0ee98dd7092ebcba6e41076285f99d0b2b0de5fe",

​      "network": "Ethereum",

​      "chainId": "1",

​      "fromAddress": "0xa9e358e33a57e67c9b84618a52f0194c345c8e35",

​      "cashierId": 86100021

​}
```

