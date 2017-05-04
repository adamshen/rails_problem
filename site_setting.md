# Site Setting
有时候需要存储和读取站点配置, 这个功能从逻辑上可以拆分成三层

![Setting](https://github.com/adamshen/rails_problem/blob/master/images/setting.png)

### Data Source
实际的存储介质，站点的配置可以存在文件里，也可以存在cache或者数据库里

- 为了便于随时修改，站点初始化时的基本配置会存在yml里
- 一些部署后会经常改变的配置，可以在数据库里建个表来存储
- 为了加快访问速度，可以缓存在cache里

### Access Control
从多个Provider中读出配置里的key-value键值对，能够实现namespace以及处理I/O错误

### Logic
根据上一层取出的value，封装一定的逻辑

显然根据SRP原则，这类需要应该独立封装在一个类里，以供其他类调用

## gem
https://github.com/huacnlee/rails-settings-cached

rails-settings-cached这个gem会根据cache-db-yml的顺序取出value，如果想封装一定的逻辑，直接把方法写在生成的Setting类里就可以了

例子的话可以看这里
https://github.com/ruby-china/homeland/blob/master/app/models/setting.rb#L33
