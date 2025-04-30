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

# 创建收币合约

收币合约分为两种类型：Web3收币合约、扫码收币合约；两种类型的合约均用于收币，区别在于收币的支付方式不同，Web3收币合约提供连接钱包授权交易的支付方式、扫码收币合约提供扫码转账的支付方式。

### 创建Web3收币合约

在收币合约--Web3收币合约页面，点击“立即创建”按钮

<figure><img src="../../../.gitbook/assets/3.png" alt=""><figcaption></figcaption></figure>

在创建Web3收币合约弹窗输入合约信息

<figure><img src="../../../.gitbook/assets/screencapture-backstage-b2b-pre-ufcfan-org-2025-04-09-15_19_26.png" alt=""><figcaption></figcaption></figure>

输入完成后点击“创建”（注意：提前准备好 200 USDT 和 足够的 Gas Fee，若 USDT 或 Gas Fee 不足则无法创建）

<figure><img src="../../../.gitbook/assets/4.png" alt=""><figcaption></figcaption></figure>

点击“创建”后会唤起钱包进行 USDT 支出授权（创建合约的服务费）

<figure><img src="../../../.gitbook/assets/5.png" alt=""><figcaption></figcaption></figure>

接着签名确认同意部署智能合约，并等待区块链确认交易完成

<figure><img src="../../../.gitbook/assets/6.png" alt=""><figcaption></figcaption></figure>

区块确认完成交易完成则Web3收币合约创建成功，弹窗关闭，在列表页面能看到刚创建的合约

<figure><img src="../../../.gitbook/assets/screencapture-backstage-b2b-pre-ufcfan-org-2025-04-09-15_41_37.png" alt=""><figcaption></figcaption></figure>

### 创建扫码收币合约

{% hint style="warning" %}
创建扫码收币合约时，需指定"授权签名地址"和关联的"资产接收地址"，即创建扫码收币合约的前提是先创建Web3收币合约。
{% endhint %}

在收币合约--扫码收币合约页面，点击“立即创建”按钮

<figure><img src="../../../.gitbook/assets/7.png" alt=""><figcaption></figcaption></figure>

在创建扫码收币合约弹窗输入合约信息

<figure><img src="../../../.gitbook/assets/screencapture-backstage-b2b-pre-ufcfan-org-2025-04-09-16_04_09.png" alt=""><figcaption></figcaption></figure>

输入完成后点击“创建”（注意：提前准备好 200 USDT 和 足够的 Gas Fee，若 USDT 或 Gas Fee 不足则无法创建）

<figure><img src="../../../.gitbook/assets/8.png" alt=""><figcaption></figcaption></figure>

点击“创建”后会唤起钱包进行 USDT 支出授权（创建合约的服务费）

<figure><img src="../../../.gitbook/assets/9.png" alt=""><figcaption></figcaption></figure>

着签名确认同意部署智能合约，并等待区块链确认交易完成

<figure><img src="../../../.gitbook/assets/10.png" alt=""><figcaption></figcaption></figure>

区块确认完成交易完成则扫码收币合约创建成功，弹窗关闭，在列表页面能看到刚创建的合约

<figure><img src="../../../.gitbook/assets/screencapture-backstage-b2b-pre-ufcfan-org-2025-04-09-19_46_55.png" alt=""><figcaption></figcaption></figure>

