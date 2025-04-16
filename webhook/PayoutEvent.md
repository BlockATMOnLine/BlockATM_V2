---
icon: plug-circle-plus
---

# Payout Event

当支付订单完成时，BlockATM将通过Webhook通知您支付事件，以便您及时获取结果。

包含的状态有：
* SUCCESS
* REFUSE

将返回以下字段：

| 字段名       | 说明                                                                                                                             | 示例值                                     |
|--------------|----------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------|
| bizOrderNo   | 商户平台订单编号                                                                                                                | B202505220012                              |
| amount       | 支付给目标地址的金额                                                                                                            | 13.410037                                  |
| chainId      | 区块链网络唯一标识符，可在[chainlist.org](https://chainlist.org/)查询                                                          | 1 (ETH主网)                                |
| custNo       | 客户编号                                                                                                                       | 860021                                     |
| fee          | 订单手续费（单位：USDT）                                                                                                        | 2                                          |
| gasAmount    | 实际Gas消耗量（单位：USDT）                                                                                                     | 1                                          |
| toAddress    | 资产转入的目标地址                                                                                                              | 0xa9e358E33a57E67c9B84618a52f0194C345C8e35 |
| network      | 交易发生的网络名称（建议使用chainId进行唯一标识）                                                                               | Ethereum                                   |
| symbol       | 支付使用的代币标识                                                                                                              | USDT                                       |
| txId         | 区块链交易哈希，可通过对应网络区块浏览器查询详情                                                                                | 0x....                                     |
| type         | 订单类型：<br>1-智能合约支付<br>2-二维码支付<br>3-智能合约地址直连                                                              | 1                                          |
| status       | 订单状态：<br>SUCCESS(成功)<br>REFUSE(拒绝)                                                                                     | SUCCESS                                    |
| createTime   | 订单创建时间（毫秒时间戳）                                                                                                      | 1693212861016                              |
| contractId   | 在BlockATM后台创建的支付合约唯一ID                                                                                              | 200910                                     |
| blockTime    | 订单上链时间（毫秒时间戳）                                                                                                      | 1693212861016                              |

Example:

```json
{
​      "custNo": "CustNo_2022140101",
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
​  }
```





