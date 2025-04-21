---
description: 仅需要三步轻松完成集成
---

# Widget SDK

#### 1. 引入SDK

将 SDK 以脚本形式添加到您的 HTML 文件中：

{% tabs %}
{% tab title="正式环境" %}
```javascript
<script src="https://pay.blockatm.net/libs/v2/BlockATM.umd.js?apiKey=[API_KEY]"></script>
```
{% endtab %}

{% tab title="沙箱环境" %}
```javascript
<script src="https://cashier-b2b-pre.ufcfan.org/libs/v2/BlockATM.umd.js?apiKey=[API_KEY]"></script>
```
{% endtab %}
{% endtabs %}

引入此脚本后，即可初始化 Web SDK 并开始集成加密货币支付解决方案。



#### 2.  参数签名

展示收银台widget前，需对参数进行签名。[了解签名机制](https://xn--d6q865b87npk2a/)。

参考代码示例：

```javascript

// 1. Prepare payment parameters
const options = {
  custNo: 'CUST123456',          // Customer ID (required)
  orderNo: '473_800000001',      // Order number 
  lang: 'en-US',                 // Language (optional, default: en-US, supports zh-CN/zh-HK/en-US)
  chainId: 1,                    // Blockchain network ID (optional, 1=ETH Mainnet)
  currency: 'USDT',              // Cryptocurrency type (optional, e.g. USDT)
  amount: 100.5,                 // Payment amount (optional)
};
​
// 2. Generate signature parameters
const { urlForSignature } = window.BlockATM.generateUrlForSigning({ 
  ...options, 
  apiKey: 'YOUR_API_KEY'         // Replace with your API key
});
​
// 3. Get signature from backend
const { signature } = await fetch("/sign-url", {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ urlForSignature }),
}).then(res => res.json());
```

查看完整的请求参数



#### 3. 初始化

在您的应用中通过以下参数初始化 SDK，唤起Blockatm收银台以及处理您的业务逻辑。

&#x20;参考代码：

```javascript
// Initialize and show cashier.
window.BlockATM.init(
  document.getElementById('blockatm-container'), // Container element
  {
    ...options,
    signature,                                  // Signature from backend
    callback: ({ type }) => {                   // Payment result callback
      switch(type) {
        case 'cancel': 
          // Handle payment cancellation
          break;
        case 'finish': 
          // Handle successful payment
          break;
      }
    }
  }
);
```



