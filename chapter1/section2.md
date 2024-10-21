# 业务逻辑梳理



### 金融平台关系展示图

其中有些关键动作是放在智能合约里实现为了在区块链上留下不可修改记录，以方便后期审计的介入。

```mermaid
	    sequenceDiagram
	    participant EA as 企业A
	    participant ClientA as 客户端A
	    participant JRC as 金融C
	    participant PRIMI as PRIMIHUB
	    participant IPFS as IPFS存储
	    participant ChainB as 其他链
	    participant BankA as 银行机构
	    
	    EA ->> ClientA: 资产数字化
	    ClientA ->>ClientA: 保存资产数据
	    ClientA ->>PRIMI: 数据加密
	    PRIMI ->>ClientA: 返回加密数据
	    ClientA ->>IPFS: 提交加密数据到IPFS
	    IPFS ->>ClientA: 返回CID
	    EA ->>ClientA: 融资申请流程
	    ClientA ->>JRC: 发起融资申请
	    ClientA ->>JRC: 提交应收账款(加密)
	    JRC ->>PRIMI: 发起多方安全计算MPC
	    PRIMI ->>IPFS: 根据CID查询内容
	    IPFS ->>PRIMI: 返回CID对应数据内容
	    Note Over PRIMI: 做验证计算
	    PRIMI ->>JRC: 返回验证结果
	    Note Over JRC: 如果验证成功
	    JRC ->>BankA: 发起授信、贷款发放
	    JRC ->>ClientA: 返回融资状态
```


## 企业AB后台功能菜单

- 用户账户管理
  - 用户注册、登录
  - 账户信息维护
  - 权限分配与角色管理

- 权限与访问控制
  - 角色定义与权限分配
  - 访问日志记录
  - 操作审计跟踪

- 合同管理
  - 合同登记
  - 合同查询

- 应收账款管理
  - 应收账款登记
  - 应收账款查询
- 应付账款管理

  - 应付账款登记
  - 应付账款查询

- 融资申请审批流程
  - 融资申请提交
  - 融资进度跟踪

- 智能合约管理
  - 智能合约部署
  - 智能合约执行
  - 智能合约审核

- IPFS集成
  - 敏感数据加密存储
  - 文件上传至IPFS
  - 文件存储管理
  - 文件检索与验证
  - 数据解密访问控制
  

- 合规性检查
  - 法规遵循性检查
  - 合规性报告生成
  - 监管机构接口

- 平台维护和管理
  - 系统性能监控
  - 交易活动监控
  - 安全事件监控

## 金融机构后台功能菜单

- 用户账户管理
  - 用户注册、登录
  - 账户信息维护
  - 权限分配与角色管理

- 权限与访问控制
  - 角色定义与权限分配
  - 访问日志记录
  - 操作审计跟踪

- 融资申请审批流程
  - 融资申请审核
  - 融资进度跟踪

- 智能合约管理
  - 智能合约部署
  - 智能合约执行
  - 智能合约审核

- 跨链交易支持
  - 跨链资产转移
  - 跨链数据同步
  - 跨链交易验证

- IPFS集成
  - 敏感数据加密存储
  - 文件上传至IPFS
  - 文件存储管理
  - 文件检索与验证
  - 数据解密访问控制

- 平台运维与管理
  - 系统性能监控
  - 交易活动监控
  - 安全事件监控

- 合规性检查
  - 法规遵循性检查
  - 合规性报告生成
  - 监管机构接口
