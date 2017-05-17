# Site Setting
## 需求
存储和读取站点配置

## 问题
- 需要支持多个Data Source(cache yml db)
- 实现namespace
- 一定的逻辑，比如某个setting是通过好几个value计算出来的

## Gems
https://github.com/huacnlee/rails-settings-cached

## 解决思路
逻辑上分为三层

![Setting](https://github.com/adamshen/rails_problem/blob/master/images/setting.png)

通常解决的办法是创建一个model，然后把Access Control和逻辑层都写到里面去。

写的时候只需要注意source的优先级即可，namespace可以直接合成到value里。
