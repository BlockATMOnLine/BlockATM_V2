---
description: Define your cashier
---

# Widget Params

## The complete set of Widget  parameters
| Parameter           | Description |
|---------------------|-------------|
| `apiKey` **required** | Your publishable API key. This is used to assign customers and transactions to your BlockATM account. |
| `custNo` **required** | Your customer code. cannot be empty. |
| `orderNo` | Your order code. |
| `chainId` | When a `chainId` is specified, customers will not be able to view or switch to other chains on the Cashier web. |
| `defaultChainId` | You can specify the chain displayed by default on the Cashier web, but customers can still choose other chains. Note: This field will be invalid when the `chainId` is specified. |
| `lang` | The BCP 47 standard language code representing the language the widget should use. If you pass a code for a language we do not support, the widget will remain in the current default language (usually the browser's language or `en-US`). |
| `currency` | The code of the cryptocurrency (e.g., USDT, USDC, DAI, [supported-cryptocurrencies](https://blockatm.readme.io/reference/supported-cryptocurrencies)) you want the customer to pay. Customers cannot select another currency. Use the codes listed in the BlockATM Dashboard to differentiate networks. |
| `defaultCurrency` | The code of the cryptocurrency you would prefer the customer to pay. Customers can still select another currency. If both `currency` and `defaultCurrency` are passed, `currency` will take precedence. You can customize the list of stablecoins displayed on the checkout page via the "TOKENS" option in your admin dashboard settings. |
| `amount` | A positive number representing how much cryptocurrency the customer wants to pay. Best used together with the `currency` parameter. |
| `redirectURL` | A URL you'd like to redirect the customer to after they complete the payment flow in the widget. We will append the transaction's ID and status as query parameters to your URL (e.g., `redirectURL?txId={{transactionId}}&status=pending`). |
| `signature` | All query parameter values (not the entire query string) need to be URL-encoded before generating the signature to ensure its validity. You need to enable this in the backend control panel. |
| `minAmount` | The minimum payment amount allowed for user input. This only applies when the `amount` field is not specified. |
| `maxAmount` | The maximum payment amount allowed for user input. This only applies when the `amount` field is not specified. |
####



