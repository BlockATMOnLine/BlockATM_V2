# Request

#### 1. Request Endpoint URLs

<table><thead><tr><th>Environment</th><th>URL</th><th data-hidden></th></tr></thead><tbody><tr><td>Production Environment</td><td><a href="https://backend.blockatm.net">https://open.blockatm.net</a></td><td></td></tr><tr><td><strong>Sandbox</strong> Environment</td><td><a href="https://backstage-b2b-pre.ufcfan.org">https://backstage-b2b-pre.ufcfan.org</a></td><td></td></tr></tbody></table>



2. #### Request  Header

## 请求头规范

每个服务端请求必须包含以下请求头：

| 参数名 | 说明 | 示例 |
|--------|------|------|
| **BlockATM-api-Key**<br><span style="color:blue">(必填)</span> | 在BlockATM控制台获取的API密钥 | `pk_payment_my3T68cbuIXf1x3QOEbWtFEfcJPxeBr8wTewDVM` |
| **BlockATM-Request-Time** | 请求发起时间戳（精确到毫秒）<br>仅需在标注<span style="color:orange">"需签名"</span>的接口添加 | `1742725435000` |
| **BlockATM-Signature-V2** | 使用密钥生成的签名<br>仅需在标注<span style="color:orange">"需签名"</span>的接口添加 | - |
| **BlockATM-Rec_Window** | 请求有效期（毫秒）<br>• 默认值：30000毫秒<br>• 最大值：60000毫秒 | `30000` |

**时效性验证规则**：
1. 服务端会校验`BlockATM-Request-Time`头
2. 当`(当前时间 - 请求时间) > Rec_Window值`时，请求将被拒绝
3. 可通过自定义`BlockATM-Rec_Window`覆盖默认值（单位：毫秒）


#### 3. 请求参数

参考具体接口说明



#### 4.返回参数




| 参数名    | 类型     | 说明                                                                 |
|-----------|----------|----------------------------------------------------------------------|
| code      | String   | 系统通用返回码。<br>200表示成功，其他值为异常状态                   |
| success   | Boolean  | 标识程序是否处理成功。<br>true-成功，false-失败                      |
| data      | Object   | 数据主体，统一返回各业务响应信息                                    |
| trace     | String   | 全局链路追踪标识信息                                                |
| msg       | String   | 程序处理后的提示信息                                                |







