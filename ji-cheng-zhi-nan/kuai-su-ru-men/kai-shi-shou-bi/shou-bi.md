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

# 收币

对接收银台后，您的用户即可在您的业务平台唤起 BlockATM 收银台进行支付

### 支付流程演示

您的用户在您的业务平台唤起 BlockATM 收银台

<figure><img src="../../../.gitbook/assets/screencapture-cashier-b2b-pre-ufcfan-org-zh-CN-2025-04-24-14_47_37.png" alt=""><figcaption></figcaption></figure>

进入 BlockATM 收银台后，选择"支付网络"、"支付币种"、"输入支付金额"

<figure><img src="../../../.gitbook/assets/screencapture-cashier-b2b-pre-ufcfan-org-zh-CN-2025-04-24-14_50_21.png" alt=""><figcaption><p>未选择支付信息</p></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (6).png" alt=""><figcaption><p>已选择支付信息</p></figcaption></figure>

接着选择支付方式

**若选择连接钱包支付**，会唤起连接钱包方式（以 MetaMask（浏览器拓展程序）为例）

<figure><img src="../../../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

选择 MetaMask 后唤起钱包进行连接，点击“连接”，同意钱包与 BlockATM收银台 连接

<figure><img src="../../../.gitbook/assets/63.png" alt=""><figcaption></figcaption></figure>

连接钱包后，收银台进入支付确认页面，点击“立即支付”开始支付流程

<figure><img src="../../../.gitbook/assets/64.png" alt=""><figcaption></figcaption></figure>

首先唤起钱包支出上限请求，点击“确认”以同意智能合约调用钱包指定数量的资产

<figure><img src="../../../.gitbook/assets/65.png" alt=""><figcaption></figcaption></figure>

接着弹出交易请求，点击“确认”进行链上交易

<figure><img src="../../../.gitbook/assets/66.png" alt=""><figcaption></figcaption></figure>

确认交易后，等待区块链确认

<figure><img src="../../../.gitbook/assets/67.png" alt=""><figcaption></figcaption></figure>

区块链确认完成后，交易完成，用户可前往区块链浏览器查看交易详情

<figure><img src="../../../.gitbook/assets/68.png" alt=""><figcaption></figcaption></figure>

**若选择扫码转账支付**

收银台进入支付确认页面，展示收款地址二维码

<figure><img src="../../../.gitbook/assets/screencapture-cashier-b2b-pre-ufcfan-org-zh-CN-2025-04-24-15_30_27.png" alt=""><figcaption></figcaption></figure>

打开钱包App扫描收款地址二维码，或者复制收款地址到钱包应用中进行转账（以 MetaMask App为例）

<figure><img src="../../../.gitbook/assets/71.png" alt=""><figcaption></figcaption></figure>

确认接收地址后，输入支付（注意：需与 BlockATM收银台的支付金额一致，否则支付无效），输入金额后点击“下一步”

<figure><img src="../../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

确认交易信息，点击“发送”

<figure><img src="../../../.gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>



发送后等待区块链确认

<figure><img src="../../../.gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/72.png" alt=""><figcaption></figcaption></figure>

注意：若钱包执行交易后收银台页面未自动跳转到结果页，可点击“我已支付”按钮

<figure><img src="../../../.gitbook/assets/73.png" alt=""><figcaption></figcaption></figure>

区块链确认完成后，交易完成，用户可前往区块链浏览器查看交易详情

<figure><img src="../../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/70.png" alt=""><figcaption></figcaption></figure>

此时您（商户）可在 BlockATM DApp 收银台模块找到对应的收银台，并点击进入订单页面查看交易记录

<figure><img src="../../../.gitbook/assets/75.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>
