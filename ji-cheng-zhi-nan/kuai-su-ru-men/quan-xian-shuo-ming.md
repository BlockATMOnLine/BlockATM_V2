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

# 权限说明

### BlockATM 平台中有三种角色地址：管理员地址、授权签名地址、运营地址

* 管理员地址：创建智能合约的钱包即为管理员，负责管理智能合约和收银台
* 授权签名地址：管理员在创建智能合约时写入合约的授权签名地址，负责合约资产的管理（包括提币、充币、付币）
* 运营地址：管理员创建收银台后可为收银台添加运营地址，负责收银台订单的运维（包括掉单处理、补发Webhook 通知）

| 模块、功能/角色   | 管理员地址 | 授权签名地址 | 运营地址 |
| ---------- | ----- | ------ | ---- |
| 钱包账户       | ✅     | ✅      | ✅    |
|     关联钱包   | ✅     | ❌      | ❌    |
|     更换钱包   | ✅     | ✅      | ✅    |
|     断开连接   | ✅     | ✅      | ✅    |
| 全局设置       | ✅     | ✅      | ✅    |
| 新手引导       | ✅     | ❌      | ❌    |
| 收币智能合约     | ✅     | ✅      | ❌    |
|     创建合约   | ✅     | ❌      | ❌    |
|     删除合约   | ✅     | ❌      | ❌    |
|     编辑合约   | ✅     | ❌      | ❌    |
|     合约列表   | ✅     | ✅      | ❌    |
| 转账智能合约     | ✅     | ✅      | ❌    |
|     创建合约   | ✅     | ❌      | ❌    |
|     删除合约   | ✅     | ❌      | ❌    |
|     编辑合约   | ✅     | ❌      | ❌    |
|     合约列表   | ✅     | ✅      | ❌    |
| 收银台        | ✅     | ❌      | ✅    |
|     创建收银台  | ✅     | ❌      | ❌    |
|     删除收银台  | ✅     | ❌      | ❌    |
|     编辑收银台  | ✅     | ❌      | ❌    |
|     启用收银台  | ✅     | ❌      | ❌    |
|     禁用收银台  | ✅     | ❌      | ❌    |
|     测试收银台  | ✅     | ❌      | ❌    |
|     对接收银台  | ✅     | ❌      | ❌    |
|     收银台列表  | ✅     | ❌      | ✅    |
|     收币订单   | ✅     | ❌      | ✅    |
|     完成订单   | ❌     | ❌      | ✅    |
|     补发通知   | ❌     | ❌      | ✅    |
|     运营列表   | ✅     | ❌      | ❌    |
|     添加运营   | ✅     | ❌      | ❌    |
|     移除运营   | ✅     | ❌      | ❌    |
|     编辑运营昵称 | ✅     | ❌      | ❌    |
| 付币智能合约     | ✅     | ✅      | ❌    |
|     创建合约   | ✅     | ❌      | ❌    |
|     删除合约   | ✅     | ❌      | ❌    |
|     编辑合约   | ✅     | ❌      | ❌    |
|     对接合约   | ✅     | ❌      | ❌    |
|     合约列表   | ✅     | ✅      | ❌    |
| 资产         | ✅     | ✅      | ❌    |
| Web3收币合约资产 | ✅     | ✅      | ❌    |
|     收币记录   | ✅     | ✅      | ❌    |
|     提币记录   | ✅     | ✅      | ❌    |
|     提币     | ❌     | ✅      | ❌    |
| 扫码收币合约资产   | ✅     | ✅      | ❌    |
|     收币记录   | ✅     | ✅      | ❌    |
|     提币记录   | ✅     | ✅      | ❌    |
|     提币     | ❌     | ✅      | ❌    |
| 付币智能合约资产   | ✅     | ✅      | ❌    |
|     付币记录   | ✅     | ✅      | ❌    |
|     补发通知   | ❌     | ✅      | ❌    |
|     充币记录   | ✅     | ✅      | ❌    |
|     付币     | ❌     | ✅      | ❌    |
|     充币     | ❌     | ✅      | ❌    |
