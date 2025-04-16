---
description: 安全支付
icon: key-skeleton-left-right
---

# 参数签名

帮助阻止未经授权的第三方访问请求。签名必须附加在URL的末尾。

**URL签名步骤**

1. 将您的支付组件里的URL参数发送到后端服务器
2. 使用BlockATM收银台中的密钥生成签名
3. 返回签名或完整的已签名URL
4. 使用SDK或URL展示支付组件

**签名生成方法**

1. 使用SHA-256哈希算法创建HMAC（基于哈希的消息认证码）
2. 使用收银台的Secret Key作为密钥
3. 使用原始URL的查询字符串作为消息内容
4. 特别注意：基于URL的集成方案中，所有查询参数值都必须先进行URL编码再生成签名



### 简单代码示例：

{% tabs %}
{% tab title="JavaScript" %}
```javascript
import crypto from 'crypto';
import { URL } from 'url';

// Configuration - replace with your actual values
const originalUrl = 'https://cashier.blockatm.com?apiKey=pck_payment_my3T68cbuIXf1x3QOEbWtFEfcJPxeBr8wTewDVM&currencyCode=eth&walletAddress=0xde0b295669a9fd93d5f28d9ec85e40f4cb697bae';
const secretKey = 'sck_QOoPSlHDSsgXYeNyTP2i0ug1HKLRjHw9Ug7mCc1Q0';

// Process URL and parameters
const urlObj = new URL(originalUrl);
const params = urlObj.searchParams;

// URL encode all parameter values
params.forEach((v, k) => params.set(k, encodeURIComponent(v)));

// Generate signature using HMAC-SHA256
const signature = crypto.createHmac('sha256', secretKey)
    .update(params.toString())
    .digest('hex');

// Append signature to URL
urlObj.searchParams.set('signature', signature);

console.log('Signed URL:\n' + urlObj.toString());
```
{% endtab %}

{% tab title="java" %}
```java
package com.btm.api.service;

import javax.crypto.Mac;
import javax.crypto.spec.SecretKeySpec;
import java.net.URI;
import java.net.URLEncoder;
import java.nio.charset.StandardCharsets;
import java.util.*;

public class UrlSigner {

    public static void main(String[] args) throws Exception {
        // Configuration
        String originalUrl = "https://cashier.blockatm.com?apiKey=pck_payment_my3T68cbuIXf1x3QOEbWtFEfcJPxeBr8wTewDVM&currencyCode=eth&walletAddress=0xde0b295669a9fd93d5f28d9ec85e40f4cb697bae";
        String secretKey = "sck_QOoPSlHDSsgXYeNyTP2i0ug1HKLRjHw9Ug7mCc1Q0";

        // Process URL and parameters
        String query = new URI(originalUrl).getQuery();
        Map<String, String> params = new LinkedHashMap<>();

        // URL encode parameters
        for (String param : query.split("&")) {
            String[] kv = param.split("=", 2);
            params.put(kv[0], kv.length > 1 ? URLEncoder.encode(kv[1], StandardCharsets.UTF_8.name()) : "");
        }

        // Build query string
        String queryString = String.join("&", params.entrySet().stream()
                .map(e -> e.getKey() + "=" + e.getValue())
                .toArray(String[]::new));

        // Generate and append signature
        String signedUrl = originalUrl + "&signature=" + hmacSha256(secretKey, queryString);
        System.out.println("Signed URL:\n" + signedUrl);
    }

    private static String hmacSha256(String secret, String data) throws Exception {
        Mac mac = Mac.getInstance("HmacSHA256");
        mac.init(new SecretKeySpec(secret.getBytes(StandardCharsets.UTF_8), "HmacSHA256"));
        return bytesToHex(mac.doFinal(data.getBytes(StandardCharsets.UTF_8)));
    }

    private static String bytesToHex(byte[] bytes) {
        StringBuilder sb = new StringBuilder();
        for (byte b : bytes) {
            sb.append(String.format("%02x", b));
        }
        return sb.toString();
    }
}
```
{% endtab %}

{% tab title="python" %}
```python
import hmac
import hashlib
from urllib.parse import urlparse, parse_qs, urlencode, quote

def sign_url():
    # Configuration
    original_url = "https://cashier.blockatm.com?apiKey=pck_payment_my3T68cbuIXf1x3QOEbWtFEfcJPxeBr8wTewDVM&currencyCode=eth&walletAddress=0xde0b295669a9fd93d5f28d9ec85e40f4cb697bae"
    secret_key = "sck_QOoPSlHDSsgXYeNyTP2i0ug1HKLRjHw9Ug7mCc1Q0"

    # Parse URL and parameters
    parsed = urlparse(original_url)
    params = parse_qs(parsed.query, keep_blank_values=True)

    # URL encode parameter values
    encoded_params = {k: quote(v[0], safe='') for k, v in params.items()}

    # Generate query string
    query_string = urlencode(encoded_params)

    # Calculate HMAC-SHA256 signature
    signature = hmac.new(
        secret_key.encode('utf-8'),
        query_string.encode('utf-8'),
        hashlib.sha256
    ).hexdigest()

    # Construct signed URL
    signed_url = f"{original_url}&signature={signature}"
    print("Signed URL:")
    print(signed_url)

if __name__ == "__main__":
    sign_url()
```
{% endtab %}

{% tab title="php" %}
```php
<?php
function signUrl() {
    // Configuration
    $originalUrl = "https://cashier.blockatm.com?apiKey=pck_payment_my3T68cbuIXf1x3QOEbWtFEfcJPxeBr8wTewDVM&currencyCode=eth&walletAddress=0xde0b295669a9fd93d5f28d9ec85e40f4cb697bae";
    $secretKey = "sck_QOoPSlHDSsgXYeNyTP2i0ug1HKLRjHw9Ug7mCc1Q0";

    // Parse URL and parameters
    $parsedUrl = parse_url($originalUrl);
    parse_str($parsedUrl['query'], $params);

    // URL encode parameter values
    $encodedParams = array_map('urlencode', $params);

    // Generate query string
    $queryString = http_build_query($encodedParams);

    // Calculate HMAC-SHA256 signature
    $signature = hash_hmac('sha256', $queryString, $secretKey);

    // Construct signed URL
    $signedUrl = $originalUrl . '&signature=' . $signature;
    echo "Signed URL:\n" . $signedUrl . "\n";
}

signUrl();
?>
```
{% endtab %}
{% endtabs %}

#### 签名示例:

**Secret Key:**

```
sk_ci_QOoPSlHDSsgXYeNyTP2i0ug1HKLRjHw9Ug7mCc1Q0
```

**URL's 参数：**

```
apiKey=pk_payment_my3T68cbuIXf1x3QOEbWtFEfcJPxeBr8wTewDVM&t=1742884523932&custNo=C86002201&orderNo=C202503225
```

**签名结果:**

```
ff7fe6e9b2d065390e325457b744a204419204f693cc42c8e079719938bc9bfd
```
