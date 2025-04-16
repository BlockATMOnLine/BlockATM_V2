# 请求

### 1.请求地址

<table><thead><tr><th>环境</th><th>请求</th><th data-hidden></th></tr></thead><tbody><tr><td><strong>生产环境</strong></td><td><a href="https://backend.blockatm.net">https://open.blockatm.net</a></td><td></td></tr><tr><td><strong>沙箱环境</strong></td><td><a href="https://backstage-b2b-pre.ufcfan.org">https://backstage-b2b-pre.ufcfan.org</a></td><td></td></tr></tbody></table>



### 2.请求头

每个服务端请求必须包含以下请求头：

| 参数名                                              | 说明                                                 | 示例                                                   |
| ------------------------------------------------ | -------------------------------------------------- | ---------------------------------------------------- |
| <p><strong>BlockATM-api-Key</strong><br>(必填)</p> | 在BlockATM控制台获取的API密钥                               | `pk_payment_my3T68cbuIXf1x3QOEbWtFEfcJPxeBr8wTewDVM` |
| **BlockATM-Request-Time**                        | <p>请求发起时间戳（精确到毫秒）<br>仅需在标注"需签名"的接口添加</p>           | `1742725435000`                                      |
| **BlockATM-Signature-V2**                        | <p>使用密钥生成的签名<br>仅需在标注"需签名"的接口添加</p>                | -                                                    |
| **BlockATM-Rec\_Window**                         | <p>请求有效期（毫秒）<br>• 默认值：30000毫秒<br>• 最大值：60000毫秒</p> | `30000`                                              |

**时效性验证规则**：

1. 服务端会校验`BlockATM-Request-Time`头
2. 当`(当前时间 - 请求时间) > Rec_Window值`时，请求将被拒绝
3. 可通过自定义`BlockATM-Rec_Window`覆盖默认值（单位：毫秒）

### 3. 请求参数

参考具体接口说明

### 4.返回参数

| 参数名     | 类型      | 说明                                     |
| ------- | ------- | -------------------------------------- |
| code    | String  | <p>系统通用返回码。<br>200表示成功，其他值为异常状态</p>    |
| success | Boolean | <p>标识程序是否处理成功。<br>true-成功，false-失败</p> |
| data    | Object  | 数据主体，统一返回各业务响应信息                       |
| trace   | String  | 全局链路追踪标识信息                             |
| msg     | String  | 程序处理后的提示信息                             |
