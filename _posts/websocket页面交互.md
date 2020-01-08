springboot项目添加spring-boot-starter-websocket依赖
添加@ServerEndpoint注解的Component实现类，内部包含@OnOpen @OnClose @OnMessage @OnError的websocket相关方法
Configuration配置ServerEndpointExporter的Bean以支持websocket
WebMvcConfigurer对应静态资源默认配置确保没被覆盖掉，直接使用静态页不经过视图解析器
resource/static下添加ws.html，host = 'ws://' + window.location.host + '/websocket'（https的对应wss）
启动应用，浏览器直接访问静态页面与后台交互