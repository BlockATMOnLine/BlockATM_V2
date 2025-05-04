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

# Operation Process

BlockATM provides two methods for uploading payout orders (API upload and Excel import). The uploaded payout orders need to be reviewed and signed by the "Authorized Signature Address" specified in the payout contract. Once confirmed, the payout orders will be processed in bulk through the BlockATM payout proxy contract, with the Gas Fee (payout funds, transaction fees, and Gas Fee on behalf of the merchant) deducted from the merchant's payout contract.

<figure><img src="../.gitbook/assets/English (3).jpg" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
The ownership of the payout contract belongs to the merchant/business, while the ownership of the payout proxy contract belongs to BlockATM. The payout proxy contract primarily provides API-based secure order uploads and Gas Fee payment on behalf of the merchant to improve payout efficiency. For more details, see:[Payout Contract](fu-bi-zhi-neng-he-yue.md)
{% endhint %}

