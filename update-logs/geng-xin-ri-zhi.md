---
description: 20250417：New Version Upgrade and Integration Guide (V2)
---

# V2.0.0

PayHelp merchants quickly locate the change points and reduce access costs.

| Module                | Old version                                                                                                                               | New version                                                                                                                                                                   | Countermeasures                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| --------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Pay by Connect Wallet | <p></p><p>Cashier accessed via widget</p><ol><li>Customer ID (custno) optional</li><li>URL parameter signature optional (ECDSA)</li></ol> | <p>Cashier accessed via widget<br>1. <code>custno</code> field now required<br>2. URL parameter signature now mandatory (HMAC-SHA256)<br>3. SDK URL updated to v2 version</p> | <p><br>1. 检查代码是否传递custNo<br>2. 集成签名逻辑（参考<a href="https://github.com/BlockATMOnLine/BlockATM_V2/blob/main/cashier/can-shu-qian-ming.md">签名文档</a>下方签名方式变更<strong>附录二</strong>）<br>3. 将sdk引入路径改为"<a href="https://cashier.blockatm.net/v2/libs/BlockATM.umd.js?apiKey=[API_KEY]">https://cashier.blockatm.net/libs</a><a href="https://cashier.blockatm.net/v2/libs/BlockATM.umd.js?apiKey=[API_KEY]">/v2</a><a href="https://cashier.blockatm.net/v2/libs/BlockATM.umd.js?apiKey=[API_KEY]">/BlockATM.umd.js?apiKey=[API_KEY]</a>"<br></p> |
| Pay by Scan-to-Pay    | Create order via API, returns cashier URL                                                                                                 | Access cashier via widget                                                                                                                                                     | <p>1. 移除原有API调用，参考新的<a href="https://github.com/BlockATMOnLine/BlockATM_V2/blob/main/quick-start/deposit-service.md">充值接入流程</a><br>2. 集成新Widget（参考<a href="https://github.com/BlockATMOnLine/BlockATM_V2/blob/main/cashier/widget-start.md">Widget集成指南</a>）</p>                                                                                                                                                                                                                                                                          |
| Payout                | <p></p><ol><li>Upload payout orders on merchant side</li></ol>                                                                            | <p>1.1. Keep merchant-side upload<br>2. Add API for creating payout orders</p>                                                                                                | 1. Choose to integrate via [Create Payout Order API](https://github.com/BlockATMOnLine/BlockATM_V2/blob/main/open-api/payout-data/create-payout-order.md)                                                                                                                                                                                                                                                                                                                                                                                |
| **Webhook**           | <p>1. ECDSA signature algorithm<br>2. Order status (int)</p>                                                                              | <p>1. Signature algorithm changed to HMAC-SHA256<br>2. Order status (String)</p>                                                                                              | <p>&#x3C;p>Signature string logic unchanged<br>1. Update verification logic (refer to &#x3C;a href="../overview/signing.md">Signature Documentation&#x3C;/a>)<br>2. Adapt to new fields (see <strong>Appendix I</strong> below)</p>                                                                                                                                                                                                                                                                                                      |
| **API**               | 路径为/api/v1                                                                                                                                | 路径升级为/api/v2                                                                                                                                                                  | See **Appendix III: API Changes**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |

***

### **Appendix I** ：Field Change Comparison (Webhook Payment)

| 旧字段           | 新字段         | 类型     | 说明                                                                                                                                                                         |
| ------------- | ----------- | ------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| platOrderNo   | `id`        | long   | BlockATM unique order identifier                                                                                                                                           |
| type          | orderType   | int    | Collection order type                                                                                                                                                      |
| status        | status      | String | <p>Change from a number to a string:<br>-1->CANCELLED<br>-1 ->EXPIRED</p><p>0-> PENDING</p><p>1-> SUCCESS</p>                                                              |
| walletAddress | fromAddress | String | The wallet address for payment.                                                                                                                                            |
| merchantId    | cashierId   | long   | The new version has registered the cashier as an access object and no longer supports merrchantId                                                                          |
| -             | blockTime   | long   | Add a new field to return the creation time of the exchange on the blockchain                                                                                              |
| fee           | -           | number | The handling fee for the new version is charged through the withdrawal contract and is fixed on a per-transaction basis. It will no longer be reflected in the collection. |
|               |             |        |                                                                                                                                                                            |

### **Appendix II**：Comparison of changes in sign

| 配置项        | 旧版本                               | 新版本         | 说明                     |
| ---------- | --------------------------------- | ----------- | ---------------------- |
| 对接公钥       | 【智能合约支付】/【二维码支付】->【设定】-【API】      | 【收银台】-【对接】  | 集成widget收银台参数、api签名请求头 |
| 对接密钥       | 【智能合约支付】/【二维码支付】->【设定】-【API】      | 【收银台】-【对接】  | 收银台参数，api参数签名密钥        |
| webhook密钥  | 【智能合约支付】/【二维码支付】->【设定】->【webhook】 | 【收银台】-【对接】  | webhook验签密钥            |
| webhook 地址 | 【智能合约支付】/【二维码支付】->【设定】->【webhook】 | 【收银台】-【对接】  | webhook 通知地址           |
| 签名规则       | 按字符&连接排序                          | 按字符&连接排序    | 签名规则未变化                |
| 签名算法       | ECDSA                             | HMAC-SHA256 |                        |

### 附录三 API 变更对照表

| API功能    | 旧版本路径/接口                                                                                          | 新版本路径/接口                        | 调整点                           |
| -------- | ------------------------------------------------------------------------------------------------- | ------------------------------- | ----------------------------- |
| 获取支持的网络  | `/api/v1/public/networklist`                                                                      | `/api/v2/pub/allNetworks`       | 如果商户在进入收银台前获取了支持的网络，请进行更新     |
| 获取支持的币种  | `/api/v1/public/cryptocurrencies`                                                                 | `/api/v2/pub/symbolList`        | 如果商户在进入收银台前获取了支持的币种，请进行更新     |
| 获取收款合约地址 | `/api/v1/contract/payment`                                                                        | `/api/v2/pub/cashier/info`      | 原独立获取收款地址，变更为通过收银台获取配置的合约地址信息 |
| 查询收款订单   | <p><code>/api/v1/payment/qrCodePayment</code><br><code>/api/v1/payment/contractPayment</code></p> | `/order/api/v2/payorder/detail` | 如果商户主动查询了收款订单状态，请变更新的API接口    |
| 查询付款订单   | `/api/v1/payout/get`                                                                              | `/api/v2/payout/detail`         | 如果商户主动查询了付款订单状态，请变更新的API接口    |

### 附录四（教学视频）：

1. 创建收币合约
2. 创建收银台
3. 收银台对接
