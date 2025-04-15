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

# 付币智能合约

付币智能合约用于批量付币，由合约中指定的财务钱包签名发起付币申请，由 BlockATM 付币代理合约执行付币操作。

{% hint style="info" %}
创建付币智能合约时，需指定财务钱包地址，一旦指定并成功创建合约后，无法修改，以此保证合约资产的安全性。
{% endhint %}

### 合约权限说明

<table><thead><tr><th width="179.046875">钱包类型</th><th>说明</th><th>权限</th></tr></thead><tbody><tr><td>管理员钱包</td><td>创建付币智能合约的钱包为管理员</td><td>删除付币智能合约</td></tr><tr><td>财务钱包</td><td>有权限使用付币智能合约资产进行付币的钱包</td><td>付币</td></tr></tbody></table>

### 付币智能合约代码

```
// Some code
```

***

### 历史合约版本

### V2

2025 年 4 月 17 日

*

### V1

2023 年 10 月 22 日

*





