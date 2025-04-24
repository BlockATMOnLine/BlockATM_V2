# 概览

Webhook是实现后台交易通知的必要机制。通过Webhook，您可以实时获取交易对象状态的异步变更通知。\
当您的账户发生任何事件时，BlockATM都会向您的应用程序发送Webhook事件通知。这对于交易状态更新等非直接API请求触发的事件尤为重要。

通知方式\
BlockATM会通过HTTP POST请求，将事件通知发送至您在账户Webhook设置中配置的所有端点URL。

### 配置需求

**商户设置步骤**:

* 在【收银台】/【【付币合约】->【对接】中设置webhook 通知地址
* 在【收银台】/【【付币合约】->【对接】中获取 【对接密钥】

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

**Request Specification**:

```http
POST /your-endpoint HTTP/1.1
Host: your-server.com
Content-Type: application/json
BlockATM-Signature-V2: ab3d...
BlockATM-Request-Time: 1743060268000
BlockATM-Event: Payment

{
  "cust_no": "evt_123456789",
  "order_no": "ORD202312345",
  "status": 9,
  "timestamp": 1743060268000
  ...
}
```

### 验证请求头

| Header                | Description                                                                             | Example       |
| --------------------- | --------------------------------------------------------------------------------------- | ------------- |
| BlockATM-Signature-V2 | HMAC-SHA256 signature of payload. [see detail.](../open-api/openapi/request-signing.md) | c3109d97...   |
| BlockATM-Request-Time | Unix timestamp (milliseconds)                                                           | 1743060268000 |
| BlockATM-Event        | Event type identifier                                                                   | payment       |

### 简单处理示例

```python
@app.route('/webhook', methods=['POST'])
def handle_webhook():
    signature = request.headers.get('BlockATM-Signature-V2')
    timestamp = request.headers.get('BlockATM-Request-Time')
    event_type = request.headers.get('BlockATM-Event')
    
    # Verify request
    if not verify_request(signature, timestamp, request.data):
        return "Invalid request", 401
    
    # Process event
    if event_type == 'payment':
        handle_payment(request.json)
    elif event_type == 'payout':
        handle_payout(request.json)
        
    return "OK", 200
```

## 重要的提醒

* 必须使用HTTPS协议的Webhook接收地址，确保数据传输安全
* 您的服务器需返回HTTP 200状态码，以确认成功接收Webhook通知
* 若Webhook投递失败，BlockATM将自动重试，建议记录所有事件日志用于故障排查
