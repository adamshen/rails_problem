# authenticate
认证几乎是大多数应用需要的功能，以至于django是直接把它集成在框架内部的，对于Rails来说，一般是用devise

https://github.com/plataformatec/devise

devise依赖了warden和bcrypt，是一个Rails engine，相当于一个直接嵌入Rails的组件

authenticate要解决的问题至少应该有这些

### 注册、登陆/登出、修改密码

devise有sessions、passwords、registrations三个controller以及一些views来完成这些功能，分别对应登陆、修改密码和注册

主要的实现方式是在controller里调env['warden']对象的方法来进行身份确认，把用户信息写在devise generator生成的user-model里

### 支持多个设备同时登陆

对于Rails来说，只管接收warden传过来的user对象，并不关心user对象是哪种warden strategy命中的

要说warden支持多个不同设备的登录，倒不如说warden支持不同strategy的登录， warden对每一次的authenticate都一视同仁

### 要能够处理验证失败的结果

_比如没通过验证就重定向到登录界面_

这个功能，主要是devise在初始化的时候，把Devise::FailureApp写入到warden的配置里。这样warden在无法命中用户身份的时候，就知道该怎么做了

### 要支持多个表的身份验证

_比如支持同时登录user和admin_

这个功能warden本来就支持多个scope

### 要支持多种验证策略

_比如先从session里找user_id，然后到header里找token等等，支持多种命中策略_

warden是根据scope的strategy列表挨个命中的，所以这个功能只需要devise提前把strategy注册好就可以了

devise定义了maps，一个map对应一个scope和一个model，每个scope都注册一样的strategy列表。对于它们strategy具体行为的不同，就用model里的dsl来指定

### security

首先是cookies有secret_key来进行加密，所以secret_key不能泄露。但就算对方能够伪造session，也还有auth_salt这一关。而用户密码的验证通过bcrypt来验证，应该即使字典泄露也不会被解出来。

### 自定义
所以devise的用法很简单，用generator生成model即可。如果非要想改变devise的行为，可以自己在user-model里写一些方法，然后修改warden的strategy列表
