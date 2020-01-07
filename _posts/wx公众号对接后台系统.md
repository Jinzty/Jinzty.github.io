登录账号扫码绑定wx号
    提供公众号二维码以关注公众号
    提供绑定当前登录账号的二维码
        已登录客户端对应应用服务器生成uuid作为key值userId作为value值，放入redis5分钟失效
        使用oauth2.0简易模式，https://open.weixin.qq.com/connect/oauth2/authorize?appid=APPID&redirect_uri=REDIRECT_URI&response_type=code&scope=SCOPE&state=STATE#wechat_redirect
        回调地址REDIRECT_URI添加uuid，状态值STATE使用uuid和userID生成的签名值
            *wx客户端扫码该二维码先到wx授权，授权成功并验证回调域名没问题后会跳转回调应用服务器地址并带上状态值STATE和授权码CODE
            应用服务器根据请求回的uuid去redis取userId，取不到则失效警告，取到则校验返回的状态值是否验签通过，不一致则验签失败，验签通过将userId放入session
            应用服务器消费授权码CODE去wx请求令牌，取回OPENID放入session，userId对应的用户信息返回给客户端展示
            wx客户端确认用户信息并选择绑定，则服务器从session中取出OPENID和userId并更新关联表做绑定处理

扫码登录（非公众号，wx开放平台注册网站应用）
    登录地址跳转至（或者js嵌入）临时登录二维码页面
        应用服务器生成uuid放入session
        跳转https://open.weixin.qq.com/connect/qrconnect?appid=APPID&redirect_uri=REDIRECT_URI&response_type=code&scope=SCOPE&state=STATE#wechat_redirect
        状态值STATE使用uuid和sessionId生成的签名值
            *wx客户端扫码授权登录后，登录页面会被wx服务器跳转回调应用服务器地址并带上状态值STATE和授权码CODE
            应用服务器取session中取uuid，取不到则失效警告，uuid和sessionId计算签名值与请求中的状态值比较，不一致则验签失败
            应用服务器消费授权码CODE去wx请求令牌，取回OPENID并根据关联表做登录处理

扫码登录test（测试公众号没有snsapi_login的SCOPE权限）
    同扫码绑定实现方式，回调地址改用登录回调，授权后直接回改缓存中状态并消费CODE取回OPENID及对应的userId放入缓存
    页面提供扫码页后定时轮询key状态(或websocket通知)，若扫码授权通过并能从缓存取到userId则登录处理，否则同步展示key状态直到失效刷新二维码

https://github.com/Jinzty/wx-demo