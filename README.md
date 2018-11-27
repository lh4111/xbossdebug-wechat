# xdebug  小程序异常监控工具

### 应用场景

1、监听线上未知错误

```
// 例如：调用错误
var result = {}
console.log(result.data.msg) // 抛出错误
```

2、记录用户操作路径，更方便重现错误

![](https://github.com/zhengguorong/xdebug-wechat/blob/master/README.png)

### 小程序使用

##### 1、引入资源

在app.js中加入dist目录下的xdebug.min.js，记得放在App对象上面

```
var xdebug = require('xdebug.min.js') // 引用xdebug
xdebug.config.key = 'maizuo' // key为自定义唯一值，用于后端记录时区分应用
xdebug.config.url = 'https://domain.com/'; // 上报服务端地址， 当配置了钉钉机器人时此url无效
// 钉钉机器人
xdebug.config.dingtalkRobot = 'xxxxx' // 钉钉机器人webhook地址
xdebug.config.dingtalkRobotMsgFormat = true // 格式化JSON输出
// 可选参数
xdebug.config.setSystemInfo = true; // 获取系统信息
xdebug.config.setLocation = true; // 获取用户位置信息
```

##### 2、测试是否正常使用

```
App({
  onLaunch: function () {
    xdebug.error('error')
  }
})
```

##### 3、控制台查看network，如果看到一个指向你配置url的请求，那就成功了。

```
// 发送的结构如下
{
    key: String // 应用唯一id
    breadcrumbs: Array // 函数执行面包线，方便用于错误重现
    error: String // 错误堆栈信息
    systemInfo: Object // 用户系统信息
    notifierVersion: String // 插件版本
    locationInfo: Object // 用户位置信息
}
```

### 高级配置

如果你的应用日志量较大，可以通过以下参数合并日志和随机抽样。

```
xdebug.config.random = 1 // 默认为1，表示100%上报，如果设置0.5，就会随机上报
xdebug.config.repeat = 5 // 重复上报次数(对于同一个错误超过多少次不上报)
xdebug.config.mergeReport = true, // mergeReport 是否合并上报， false 关闭， true 启动（默认）
xdebug.config.except = [ /^Script error\.?/, /^Javascript error: Script error\.? on line 0/ ], // 忽略某个错误
```

### 二次开发

##### 1**、安装依赖**

```
// 进入项目目录安装依赖
npm install
// 安装rollup，用于js编译打包
npm install -g rollup
```

##### 2、开发模式 （监听代码变化，生成xdebug.js）

```
npm run watch
```

##### 3、编译（生成xdebug.min.js）

```
npm run build
```

### [方案设计思想](https://github.com/zhengguorong/xdebug/blob/master/design.md)
