# authenticate
## 需求
用户文件上传

## 问题
- 要实现文件和所属model的映射关系
- 根据大小、后缀名等进行filter
- 实现云、文件等多种存储方式
- 对文件进行预处理

## gems
https://github.com/carrierwaveuploader/carrierwave

### 要实现文件和所属model的映射关系
写一个module，include到active_record，然后在需要的model用dsl进行mount

### 根据大小、后缀名等进行filter
利用ActiveSupport::Callbacks的before和after来注册filter方法

### 实现云、文件等多种存储方式
建立一个存储抽象层

### 对文件进行预处理
用minimagic等工具来处理图片
