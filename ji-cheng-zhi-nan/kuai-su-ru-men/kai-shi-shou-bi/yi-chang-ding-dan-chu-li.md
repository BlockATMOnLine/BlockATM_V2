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

为此需要对上述两类异常订单进行处理，有权限进行一场订单的用户为运营用户，需要管理员在收银台添加

### 添加收银台运营用户



### 掉单--完成交易



### 通知异常--补发通知



