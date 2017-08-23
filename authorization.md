# authorization
## 需求
用户权限管理

## 问题
- 灵活的设置和校验权限
- 与rails的结合

## gems
https://github.com/CanCanCommunity/cancancan

### 灵活的设置和校验权限
用Strategy的设计模式，每个设置权限的dsl生成一个rule对象，然后校验的时候查找匹配的策略并返回结果

### 与rails的结合
主要是在controller增加几个help方法，来对标准的crud进行权限检查，然后为针对model的rule增加一些标准的condition描述。
