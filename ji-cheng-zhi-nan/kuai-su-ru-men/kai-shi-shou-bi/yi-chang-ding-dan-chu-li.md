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

# 异常订单处理

一般情况下收币订单状态都能正常流转，但也有小概率发生异常情况：

* **掉单：**&#x7528;户已完成支付，但订单状态未流转为成功
* **通知异常：**&#x8BA2;单的 Webhook 通知已发送但商户未收到

为此需要对上述两类异常订单进行处理，而只有收银台的运营地址有权限对异常订单进行操作，运营地址需要管理员在收银台添加

### 添加收银台运营用户

管理员地址在收银台列表，点击“运营”按钮，打开添加运营弹窗

<figure><img src="../../../.gitbook/assets/76.png" alt=""><figcaption></figcaption></figure>



### 掉单--完成交易



### 通知异常--补发通知



