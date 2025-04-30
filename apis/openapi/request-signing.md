# 签名

请求需附带签名发送至BlockATM服务器。每个请求必须在BlockATM-API-Key请求头中包含api-key，以便BlockATM验证请求确实由您的服务器发出，而非第三方。

### Basic Information

* **ApiKey** 对接公钥，需包含在每个请求的 **BlockATM-API-Key** 请求头中
* **Secret Key** 对接密钥，用于加密请求参数，生成的签名结果需放入 **BlockATM-Signature-V2** 请求头

您可以在商户后台的不同集成场景下（如收银台或付币合约）**对接**页面找到对应的API密钥。

<figure><img src="../../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

### Sign

**步骤 1**;

将JSON对象中的所有参数按照键的ASCII字符顺序升序排列，并以"key=value"的格式用"&"符号连接。

请求原始参数：

```
{"custNo":"86000123","orderNo":"202504001399","lang":"zh-CN"}
```

处理结果：

```
custNo=86000123&lang=zh-CN&orderNo=202504001399
```

**步骤 2**

将请求头中的 'BlockATM-Request-Time' 以 '\&time=' 格式拼接到字符串末尾。

假设 **BlockATM-Request-Time =** 1742723373000

处理结果:

```
custNo=86000123&lang=zh-CN&orderNo=202504001399&time=1742723373000
```

**步骤 3**

使用SHA-256哈希函数计算HMAC签名，并以您的收银台密钥(Secret Key)作为加密密钥。

```
Dihl6oOt5UkaHo9sEouquP3EqbukLX2dAOoKTSGicYryTvH1m9r6vtSLHGutZn7u34/06gjhdpbXRFPdjb51GVHvG75qWXZ1P/boL89xtuja6eTEy9q/aS8R270Q1A+m/MOTxdiifCy0IByrSpCs4VJKaj2d8jlJo2GHznsH+q0=
```

**您可以根据开发语言参考以下示例：**

{% tabs %}
{% tab title="java" %}
```java
import javax.crypto.Mac;
import javax.crypto.spec.SecretKeySpec;
import java.net.URLEncoder;
import java.nio.charset.StandardCharsets;
import java.security.InvalidKeyException;
import java.security.NoSuchAlgorithmException;
import java.util.*;

public class BlockATMSigner {
    
    // 配置参数（实际使用应从安全存储获取）
    private static final String API_KEY = "your_api_key_here";
    private static final String SECRET_KEY = "your_secret_key_here";
    private static final long REQUEST_TIME = 1742723373000L; // 示例时间戳

    public static void main(String[] args) {
        try {
            // 1. 准备请求参数
            Map<String, String> params = new HashMap<>();
            params.put("amount", "44");
            params.put("bizOrderNo", "B234569885XASA953ASDSAD");
            params.put("chainId", "11155111");
            params.put("custNo", "473_860001");
            params.put("merchantId", "286000260");
            params.put("symbol", "USDT");
            params.put("toAddress", "0xc87dd49427a188bf2b601c1d5cd2aaf36bd553d2");
            params.put("remark", "demo for create payout order");

            // 2. 生成签名
            String signature = generateSignature(params, REQUEST_TIME);

            // 3. 构建请求头
            Map<String, String> headers = new HashMap<>();
            headers.put("BlockATM-API-Key", API_KEY);
            headers.put("BlockATM-Signature-V2", signature);
            headers.put("BlockATM-Request-Time", String.valueOf(REQUEST_TIME));

            // 输出结果
            System.out.println("Request Headers:");
            headers.forEach((k, v) -> System.out.println(k + ": " + v));

        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    /**
     * 生成HMAC-SHA256签名
     */
    private static String generateSignature(Map<String, String> params, long timestamp) 
        throws NoSuchAlgorithmException, InvalidKeyException {
        
        // 步骤1：排序并拼接参数
        String sortedParams = getSortedParams(params);
        
        // 步骤2：拼接时间戳
        String payload = sortedParams + "&time=" + timestamp;
        
        // 步骤3：计算HMAC
        Mac sha256Hmac = Mac.getInstance("HmacSHA256");
        SecretKeySpec secretKey = new SecretKeySpec(
            SECRET_KEY.getBytes(StandardCharsets.UTF_8), 
            "HmacSHA256"
        );
        sha256Hmac.init(secretKey);
        byte[] hash = sha256Hmac.doFinal(payload.getBytes(StandardCharsets.UTF_8));
        
        return bytesToHex(hash);
    }

    /**
     * 按ASCII顺序排序参数
     */
    private static String getSortedParams(Map<String, String> params) {
        // 使用TreeMap自动排序
        Map<String, String> sortedMap = new TreeMap<>(params);
        
        // 拼接键值对
        List<String> paramList = new ArrayList<>();
        for (Map.Entry<String, String> entry : sortedMap.entrySet()) {
            paramList.add(entry.getKey() + "=" + entry.getValue());
        }
        return String.join("&", paramList);
    }

    /**
     * 字节数组转十六进制
     */
    private static String bytesToHex(byte[] bytes) {
        StringBuilder result = new StringBuilder();
        for (byte b : bytes) {
            result.append(String.format("%02x", b));
        }
        return result.toString();
    }
}
```
{% endtab %}

{% tab title="python" %}
```python
import hmac
import hashlib
from urllib.parse import parse_qs, urlencode

def generate_signature():
    # Configuration
    api_key = "pck_payment_my3T68cbuIXf1x3QOEbWtFEfcJPxeBr8wTewDVM"
    secret_key = "sck_QOoPSlHDSsgXYeNyTP2i0ug1HKLRjHw9Ug7mCc1Q0"
    request_time = 1743060268000

    # Sample parameters (should come from request body)
    params = {
        "amount": "44",
        "bizOrderNo": "B234569885XASA953ASDSAD",
        "chainId": "11155111",
        "custNo": "473_860001",
        "merchantId": "286000260",
        "symbol": "USDT",
        "toAddress": "0xc87dd49427a188bf2b601c1d5cd2aaf36bd553d2",
        "remark": "demo for create payout order"
    }

    # Step 1: Sort parameters by key
    sorted_params = sorted(params.items())
    query_string = urlencode(sorted_params)

    # Step 2: Add timestamp
    payload = f"{query_string}&time={request_time}"

    # Step 3: Compute HMAC-SHA256
    signature = hmac.new(
        secret_key.encode('utf-8'),
        payload.encode('utf-8'),
        hashlib.sha256
    ).hexdigest()

    # Prepare headers
    headers = {
        "BlockATM-API-Key": api_key,
        "BlockATM-Signature-V2": signature,
        "BlockATM-Request-Time": str(request_time)
    }

    print("Request Headers:")
    for k, v in headers.items():
        print(f"{k}: {v}")

if __name__ == "__main__":
    generate_signature()
```
{% endtab %}

{% tab title="PHP" %}
```php
<?php
function generateSignature() {
    // Configuration
    $apiKey = "pck_payment_my3T68cbuIXf1x3QOEbWtFEfcJPxeBr8wTewDVM";
    $secretKey = "sck_QOoPSlHDSsgXYeNyTP2i0ug1HKLRjHw9Ug7mCc1Q0";
    $requestTime = 1743060268000;

    // Sample parameters
    $params = [
        "amount" => "44",
        "bizOrderNo" => "B234569885XASA953ASDSAD",
        "chainId" => "11155111",
        "custNo" => "473_860001",
        "merchantId" => "286000260",
        "symbol" => "USDT",
        "toAddress" => "0xc87dd49427a188bf2b601c1d5cd2aaf36bd553d2",
        "remark" => "demo for create payout order"
    ];

    // Step 1: Sort parameters
    ksort($params);
    $queryString = http_build_query($params);

    // Step 2: Add timestamp
    $payload = $queryString . "&time=" . $requestTime;

    // Step 3: Compute HMAC-SHA256
    $signature = hash_hmac('sha256', $payload, $secretKey);

    // Prepare headers
    $headers = [
        "BlockATM-API-Key: " . $apiKey,
        "BlockATM-Signature-V2: " . $signature,
        "BlockATM-Request-Time: " . $requestTime
    ];

    echo "Request Headers:\n";
    foreach ($headers as $header) {
        echo $header . "\n";
    }
}

generateSignature();
?>

```
{% endtab %}

{% tab title="C++" %}
```cpp
#include <iostream>
#include <map>
#include <string>
#include <openssl/hmac.h>
#include <iomanip>
#include <sstream>

std::string hmac_sha256(const std::string& key, const std::string& data) {
    unsigned char hash[EVP_MAX_MD_SIZE];
    unsigned int len = 0;
    
    HMAC_CTX* ctx = HMAC_CTX_new();
    HMAC_Init_ex(ctx, key.c_str(), key.length(), EVP_sha256(), NULL);
    HMAC_Update(ctx, (unsigned char*)data.c_str(), data.length());
    HMAC_Final(ctx, hash, &len);
    HMAC_CTX_free(ctx);

    std::stringstream ss;
    for(unsigned int i = 0; i < len; i++) {
        ss << std::hex << std::setw(2) << std::setfill('0') << (int)hash[i];
    }
    return ss.str();
}

int main() {
    // Configuration
    const std::string api_key = "pck_payment_my3T68cbuIXf1x3QOEbWtFEfcJPxeBr8wTewDVM";
    const std::string secret_key = "sck_QOoPSlHDSsgXYeNyTP2i0ug1HKLRjHw9Ug7mCc1Q0";
    const long request_time = 1743060268000;

    // Sample parameters
    std::map<std::string, std::string> params = {
        {"amount", "44"},
        {"bizOrderNo", "B234569885XASA953ASDSAD"},
        {"chainId", "11155111"},
        {"custNo", "473_860001"},
        {"merchantId", "286000260"},
        {"symbol", "USDT"},
        {"toAddress", "0xc87dd49427a188bf2b601c1d5cd2aaf36bd553d2"},
        {"remark", "demo for create payout order"}
    };

    // Step 1: Sort parameters
    std::stringstream query_stream;
    for(auto it = params.begin(); it != params.end(); ++it) {
        if(it != params.begin()) query_stream << "&";
        query_stream << it->first << "=" << it->second;
    }

    // Step 2: Add timestamp
    std::string payload = query_stream.str() + "&time=" + std::to_string(request_time);

    // Step 3: Compute HMAC
    std::string signature = hmac_sha256(secret_key, payload);

    // Prepare headers
    std::cout << "Request Headers:" << std::endl;
    std::cout << "BlockATM-API-Key: " << api_key << std::endl;
    std::cout << "BlockATM-Signature-V1: " << signature << std::endl;
    std::cout << "BlockATM-Request-Time: " << request_time << std::endl;

    return 0;
}
```
{% endtab %}

{% tab title="Go" %}
```go
package main

import (
	"crypto/hmac"
	"crypto/sha256"
	"encoding/hex"
	"fmt"
	"sort"
	"strings"
)

func main() {
	// Configuration
	apiKey := "pck_payment_my3T68cbuIXf1x3QOEbWtFEfcJPxeBr8wTewDVM"
	secretKey := "sck_QOoPSlHDSsgXYeNyTP2i0ug1HKLRjHw9Ug7mCc1Q0"
	requestTime := int64(1743060268000)

	// Sample parameters
	params := map[string]string{
		"amount":      "44",
		"bizOrderNo":  "B234569885XASA953ASDSAD",
		"chainId":     "11155111",
		"custNo":      "473_860001",
		"merchantId":  "286000260",
		"symbol":      "USDT",
		"toAddress":   "0xc87dd49427a188bf2b601c1d5cd2aaf36bd553d2",
		"remark":      "demo for create payout order",
	}

	// Step 1: Sort keys
	keys := make([]string, 0, len(params))
	for k := range params {
		keys = append(keys, k)
	}
	sort.Strings(keys)

	// Build query string
	var queryParts []string
	for _, k := range keys {
		queryParts = append(queryParts, fmt.Sprintf("%s=%s", k, params[k]))
	}
	queryString := strings.Join(queryParts, "&")

	// Step 2: Add timestamp
	payload := fmt.Sprintf("%s&time=%d", queryString, requestTime)

	// Step 3: Compute HMAC
	h := hmac.New(sha256.New, []byte(secretKey))
	h.Write([]byte(payload))
	signature := hex.EncodeToString(h.Sum(nil))

	// Prepare headers
	fmt.Println("Request Headers:")
	fmt.Printf("BlockATM-API-Key: %s\n", apiKey)
	fmt.Printf("BlockATM-Signature-V1: %s\n", signature)
	fmt.Printf("BlockATM-Request-Time: %d\n", requestTime)
}
```
{% endtab %}
{% endtabs %}
