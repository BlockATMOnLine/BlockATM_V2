# 收币参数

当支付订单状态发生变更时，BlockATM将通过Webhook支付事件发送通知。包含以下状态类型：

PENDING（处理中）：当支付订单已达到要求的链上确认数

SUCCESS（支付成功）：当支付订单被确认已成功支付

EXPIRED（已过期）：因未完成支付导致客户订单超时

CANCELLED（已取消）：当客户主动取消支付订单

BlockATM将通过您配置的Webhook URL向服务器发送以下字段数据：

| 字段名         | 说明                                                                                                                                                | 示例值                                        |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------ |
| id          | BlockATM平台对应的订单编号                                                                                                                                 | 8210000374                                 |
| orderNo     | (可选) 您平台对应的订单号                                                                                                                                    | O20231022                                  |
| chainId     | 区块链网络唯一标识符，可在[chainlist.org](https://chainlist.org/)查询                                                                                            | 1                                          |
| cashierId   | 收款收银台ID                                                                                                                                           | 199                                        |
| amount      | 客户实际支付金额                                                                                                                                          | 13.410037                                  |
| custNo      | (可选) 支付订单的客户编号                                                                                                                                    | 860021                                     |
| network     | 交易发生的网络名称（建议使用chainId进行唯一标识）                                                                                                                      | Ethereum                                   |
| symbol      | 客户支付的代币标识                                                                                                                                         | USDT                                       |
| txId        | 区块链上对应的交易哈希，可通过对应网络的区块浏览器查看详情                                                                                                                     | 0x....                                     |
| orderType   | <p>订单类型：<br>1-智能合约支付<br>2-二维码支付<br>3-智能合约地址直连</p>                                                                                                 | 1                                          |
| status      | <p>订单状态：<br><strong>CANCELLED</strong>(已取消)<br><strong>EXPIRED</strong>(已过期)<br><strong>PENDING</strong>(处理中)<br><strong>SUCCESS</strong>(成功)</p> | **SUCCESS**                                |
| fromAddress | 支付地址（未支付时为空）                                                                                                                                      | 0xa9e358E33a57E67c9B84618a52f0194C345C8e35 |
| blockTime   | 订单上链时间戳                                                                                                                                           | 1693212861016                              |

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
