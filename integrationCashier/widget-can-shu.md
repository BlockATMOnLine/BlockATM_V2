---
description: 定义您的收银台
---

# Widget 参数

## 支付组件参数全集

| 参数                                                  | 类型     | 说明                                                                             |
| --------------------------------------------------- | ------ | ------------------------------------------------------------------------------ |
| <p><code>apiKey</code><br><strong>（必填）</strong></p> | string | <p> 对接Key，从商户收银台对接信息中获取<br>示例：<code>pk_prod_xxxxxx</code></p>                  |
| <p><code>custNo</code><br><strong>（必填）</strong></p> | string | <p>客户唯一标识<br>示例：<code>user_123456</code></p>                                   |
| `orderNo`                                           | string | <p>商户订单编号，建议包含时间戳<br>示例：<code>order_20231201_001</code></p>                    |
| `chainId`                                           | string | 指定支付网络（1=ETH主网, 11155111=ETH测试网）设置后用户不可切换链。 查看完整网络列表                           |
| `defaultChainId`                                    | number | <p>默认显示网络（用户可手动切换）<br>与<code>chainId</code>同时设置时无效</p>                         |
| `lang`                                              | string | <p>界面语言代码，默认<code>en-US</code><br>可选：<code>zh-CN</code>/<code>zh-HK</code></p> |
| `currency`                                          | string | <p>指定支付币种（需全大写）<br>示例：<code>USDT</code>    </p>                                |
| `defaultCurrency`                                   | string | <p>推荐支付币种（用户可切换）<br>与<code>currency</code>冲突时后者优先</p>                          |
| `amount`                                            | number | <p>指定支付金额（支持小数）<br>示例：<code>99.99</code></p>                                   |
| `redirectURL`                                       | string | <p>支付完成跳转地址（需HTTPS）<br>自动附加<code>txId</code>和<code>status</code>参数</p>         |
| `signature`                                         | string | <p><strong>（必填）</strong> 参数值需先URL编码再签名<br>格式：64位hex字符串</p>                     |
| `minAmount`                                         | number | <p>用户可输入的最小金额<br>（仅在未设置<code>amount</code>时生效）</p>                             |
| `maxAmount`                                         | number | <p>用户可输入的最大金额<br>（仅在未设置<code>amount</code>时生效）</p>                             |

####



