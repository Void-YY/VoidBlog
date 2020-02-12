# JSON Server

为什么会使用这个工具。 

- 因为在做某个大型项目时，测试环境分为4个。数据库表和数据量差异巨大。测试组同时提出bug时，在Local环境很难模拟同样的数据返回（服务端行为）。同时，线上环境又因为编译后很难debug。解决BUG就变得成本巨大。
- 做前端覆盖率测试时，很多数据依赖后端来模拟返回值触发相应的代码，而后端表关联复杂很难造出相同的数据，调试困难。

> 这时我想到能不能只模拟返回后端的Json数据，来模拟后端的行为，然后就在github上发现了这个强大的项目。它可以
    
1. *轻松的把后端的API接口mock掉，也不需要频繁的修改和配置。*  
2. *不需要修改前端任何代码，就可以获得你想要的任何数据，触发每一种条件。*  
3. *支持逻辑判断，可以做很多功能扩展（假后台假到和真的一样）。*


比如第一次请求 接口1，返回
```json
{
    result:"第一次返回"
}
```

第二次请求 接口1，返回

```json
{
    result:"第二次返回"
}
```

------------------------------------------
以下为我当时的个人配置
>package.json (启动命令定义)
```json
{
    "scripts": {
        "mock": "json-server -c json-server.json db.json"
    },
    "dependencies": {
        "json-server": "^0.15.1"
    }
}
```
>middls.js (中间件实现逻辑)
```javascript
//middle.js
module.exports = (req, res, next) => {
    // 项目需求，把所有post请求转成get接收
    if (req.method === 'POST') {
        req.method = 'GET';
    }
    // 登录的api 的路由需要特殊处理
    if (req.body
        && req.body.hasOwnProperty('logicId')
        && !req.url.endsWith('login')
    ) {
        req.url = '/' + req.body.logicId
    }
    next();
};
```
>json-server.json(库的配置)
```json
{
    // 端口号
    "port": 53000,
    // 监听文件修改
    "watch": true,
    // 使用的中间件
    "middlewares": "middle.js",
    // 记录日志
    "logger": true,
    // 静默日志输出
    "quiet": false
}
```
>db.json
```json
{
    "api1":{
        "test":"test"
    },
    "api2":{
        "test2":"test2"
    }
}
```
