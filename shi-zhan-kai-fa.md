本指南以一个简单的网络投票系统为例，详细讲述完整dapp的开发流程。

* ##### 初始化本地开发环境

首先我们根据**环境搭建**一章的内容，在本机上安装并成功启动Longclaw。

* ##### 初始化项目结构

当掌握了robin工具的使用方法之后，我们在任意目录下新建一个空目录vote，然后进到vote下，执行

`robin init -c Vote`

此时出现模板选择界面，由于网络投票需要面向的是普通用户，所以提供一个友好的交互界面是非常有必要的，因此这里选择一个开发者熟悉的前端框架，本文示例代码使用的是vue-boilerplate.

* ##### 修改上链配置信息

Robin生成的默认与链交互的U3.js配置信息在根目录下的config.js文件中。如果你是基于Longclaw构建的本地开发环境，那么你不需要做任何修改。如果你要基于线上测试网环境做开发，那么你需要修改为如下配置：

```
const config = {
  httpEndpoint: 'http://benyasin.s1.natapp.cc',
  httpEndpoint_history: 'http://history.natapp1.cc',
  broadcast: true,
  debug: false,
  verbose: false,
  sign: true,
  logger: {
    log: console.log,
    error: console.error,
    debug: console.log
  },
  chainId: '262ba309c51d91e8c13a7b4bb1b8d25906135317b09805f61fcdf4e044cd71e8',
  keyProvider: '改为你申请的测试网账号的私钥',
  binaryen: require('binaryen')
};
module.exports = config;
```

而对应的账号与可用资源需要你在测试网络申请，相关操作可以参照[测试网开发配置指南](https://developer.ultrain.io/tutorial/testnet_guide)。

