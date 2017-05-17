# authenticate
## 需求
用户身份验证

## 问题
- 要实现注册、登陆／登出、修改密码
- 多种验证策略
- 支持多个设备同时登陆(多浏览器、多设备)
- 验证失败以后的行为处理(跳到注册页面，提示密码错误)
- 支持多个表的身份验证
- security

## gems
https://github.com/plataformatec/devise

## 解决思路
![devise](https://github.com/adamshen/rails_problem/blob/master/images/devise.png)

### 注册、登陆/登出、修改密码
注册、登陆、修改密码都必须要有相应的页面，所以要写controller以及view。注册和修改密码都只是写user表的内容，登陆和登出则需要交给其他的service来处理。

devise有sessions、passwords、registrations三个controller来完成这些功能，分别对应登陆、修改密码和注册。注册和修改密码会去写user-model，登陆和登出则会交给warden去处理。

### 多种验证策略
用一个strategy array来实现，写一个父类，把request传给它，并定义好strategy的接口和返回类型。不同的strategy都继承这个类，然后实现自己的验证逻辑。

warden是根据scope的strategy列表挨个命中的，devise在初始化好之后，会在warden注册自己的strategy列表。

### 支持多个设备同时登陆
每次通过用户名密码登陆以后，发token或者加密后的cookies给client。

controller只管接收warden传过来的user对象，并不关心user对象是哪种warden strategy命中的，要说warden支持多个不同设备的登录，倒不如说warden支持不同strategy的登录， warden对每一次的authenticate都一视同仁。

### 验证失败以后的行为处理
strategy跑完之后，检查到底为何验证失败，然后返回对应的结果。

devise在初始化的时候，会把Devise::FailureApp写入到warden的配置里。这样warden在无法命中用户身份的时候，就知道该怎么做了。

### 要支持多个表的身份验证
namespace

### security
不存密码明文，密码加密后用bcrypt来验证。将cookies用secret_key + auth_salt来进行加密解密。
