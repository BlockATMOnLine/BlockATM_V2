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

# 如何创建 TronLink 钱包

{% hint style="warning" %}
由于与 Tron 网络钱包的兼容性问题，部分钱包可能无法通过 WalletConnect 连接 TronLink App。因此在 Tron 网络钱包升级并解决兼容性问题之前，BlockAT M 推荐使用 TronLink 浏览器拓展程序连接，以下仅介绍 TronLink 浏览器拓展程序的使用教程。
{% endhint %}

{% stepper %}
{% step %}
### 下载  TronLink 插件

* 访问 TronLink 官方网站（[https://www.tronlink.org/](https://www.tronlink.org/)）。
* 点击“下载”，选择适用于你的浏览器版本（仅支持 Chrome、Edge）。
* 在浏览器应用商店中点击“添加至浏览器”或类似按钮，安装扩展。
{% endstep %}

{% step %}
### 打开 TronLink

* 安装完成后，点击浏览器右上角的 TronLink 图标。
* 首次打开时，点击“创建钱包”。
{% endstep %}

{% step %}
### 设置钱包名称和密码

* 输入一个钱包名称（可选，便于识别）。
* 设置一个安全的密码（至少 8 位，建议包含字母、数字和符号），用于解锁钱包。
* 点击“下一步”。
{% endstep %}

{% step %}
### 备份助记词

* TronLink 会生成一个 12 个单词的助记词（Mnemonic Phrase），这是恢复钱包的关键。
* 点击显示助记词，将其抄写下来并妥善保存（建议手写在纸上，不要截图或存储在联网设备上）。
* 按顺序输入这些单词进行验证，完成后点击“确认”。
{% endstep %}

{% step %}
### 完成创建

* 设置完成后，你会看到钱包主界面，显示你的钱包地址（以“T”开头的一串字符）。
* 此时，钱包已创建完成，默认连接到 TRON 主网。
{% endstep %}
{% endstepper %}

### 注意事项

* 安全性：助记词是恢复钱包的唯一凭证，丢失后无法找回，泄露则可能导致资产被盗。务必妥善保管。
* 资金添加：创建钱包后，地址是空的，你需要从交易所或其他钱包转入 TRX 或其他代币才能使用。
* 用途：TronLink 可用于存储 TRX、TRC-10、TRC-20 代币，并与 TRON 生态中的 DApp（去中心化应用）交互。
