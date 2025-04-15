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

# 提币

收银台收到的加密货币托管在绑定的收币智能合约，若要将资产从收币智能合约中提出，需要合约指定的财务钱包发起提币，提取到合约指定的提币接收钱包（财务钱包和提币接收钱包权限说明见：[收币智能合约](../../../ye-wu-shuo-ming/an-quan-shou-bi/shou-bi-zhi-neng-he-yue.md#he-yue-quan-xian-shuo-ming)）

### 支付智能合约提币

首先连接财务钱包，财务在资产模块--支付智能合约--能见到 "提币" 按钮，点击 "提币"

<figure><img src="../../../.gitbook/assets/31.png" alt=""><figcaption></figcaption></figure>

提币弹窗中展示支付智能合约各代币的资产数量以及提币的手续费

<figure><img src="../../../.gitbook/assets/32.png" alt=""><figcaption></figcaption></figure>

要提取的代币输入提币数量，并选择提币接收钱包地址，此时 "提币" 按钮高亮，接着点击 "提币"

<figure><img src="../../../.gitbook/assets/33.png" alt=""><figcaption></figcaption></figure>

点击 "提币" 会唤起钱包签名授权交易

<figure><img src="../../../.gitbook/assets/34.png" alt=""><figcaption></figcaption></figure>

授权后交易执行，等待区块链确认

<figure><img src="../../../.gitbook/assets/screencapture-backstage-b2b-pre-ufcfan-org-assets-2025-04-10-20_15_58.png" alt=""><figcaption></figcaption></figure>

区块确认完成则提币成功

***

### 转账智能合约提币

首先连接财务钱包，财务在资产模块--转账智能合约--能见到 "提币" 按钮，点击 "提币"

<figure><img src="../../../.gitbook/assets/35.png" alt=""><figcaption></figcaption></figure>

提币弹窗中展示转账智能合约各代币的资产数量以及提币的手续费

<figure><img src="../../../.gitbook/assets/screencapture-backstage-b2b-pre-ufcfan-org-assets-2025-04-10-20_25_19.png" alt=""><figcaption></figcaption></figure>

勾选要提取的代币（默认提取全部数量），并选择提币接收钱包地址，此时 "提币" 按钮高亮，接着点击 "提币"

<figure><img src="../../../.gitbook/assets/36.png" alt=""><figcaption></figcaption></figure>

点击 "提币" 会唤起钱包签名授权交易

<figure><img src="../../../.gitbook/assets/38.png" alt=""><figcaption></figcaption></figure>

授权后交易执行，等待区块链确认

<figure><img src="../../../.gitbook/assets/39.png" alt=""><figcaption></figcaption></figure>

区块确认完成则提币成功

<figure><img src="../../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>
