## 一、MacOS下搭建Longclaw环境

我们推荐首选的操作系统为MacOS，采用改系统进行开发你将获得极佳的用户体验，
其次是Linux。Windows由于和Docker的兼容问题较大，不建议用于搭建共识开发环境。

MacOS下可以使用集成开发工具Longclaw，作为本地开发的链环境使用。Longclaw是一
个用于ULtrain开发的个人链环境，可用于部署合约、开发应用程序和测试运行。
那Longclaw到底有哪些作用呢？   

>+ 自动模拟8个测试账户。  
+ 实时日志分析和事务跟踪。  
+ 自动化集成本地Docker环境。

### Docker 下载
 
Longclaw是基于Docker来构建的，所以需要你在本机上提前安装并启动Docker，
关于Docker安装及使用我们提供了如下两种方式:
> * 可以参考阮一峰的[Docker入门教程](http://www.ruanyifeng.com/blog/2018/02/docker-tutorial.html)。
* 从docker官方网站下载 docker进行安装（地址：[`https://docs.docker.com/docker-for-mac/install/`](https://docs.docker.com/docker-for-mac/install/)）

### Longclaw的使用步骤

- 下载安装Longclaw，从开发者网站[下载Longclaw](https://developer.ultrain.io/tools);
- 启动Longclaw，点击mac应用中心的Longclaw应用，即可启动;

> 注意：Longclaw第一次初始化环境可能要花费几分钟，请您耐心等待，当出现以下界面，则说明Longclaw
> 已经成功帮你构建了共识网络环境，也就是本地开发环境。

​![longclaw2](https://user-images.githubusercontent.com/1866848/59199042-9b31dd80-8bc7-11e9-87cb-59254a91a764.png)

此时，你也可以直接在浏览器中输入 [`http://127.0.0.1:8888/v1/chain/get_chain_info`](http://127.0.0.1:8888/v1/chain/get_chain_info)，来查看链的信息。

![chain_info](https://user-images.githubusercontent.com/1866848/59199390-683c1980-8bc8-11e9-8988-38b74e4d1a25.jpeg)

## 二、Linux系统下搭建Longclaw环境

Linux下的共识网络环境同样依赖于Docker，关于Linux下的docker环境配置，请参照docker的官方文档，例如ubuntu下docker安装文档为：[`https://docs.docker.com/install/linux/docker-ce/ubuntu/`](https://docs.docker.com/install/linux/docker-ce/ubuntu/)。

**操作步骤：**

  - 下载U3.js源码`git clone https://github.com/ultrain-os/u3.js.git`;
  
  - 进入U3.js/docker目录，执行`cd ./u3.js/docker`;
  
  - 下载ultraind镜像并启动，执行`./start.sh`;
  
如果命令行输出以下内容，则证明本地镜像启动成功。

![linux_start](https://user-images.githubusercontent.com/1866848/59198819-31b1cf00-8bc7-11e9-8c81-8ece10e90748.jpeg)

此时，你也可以直接在浏览器中输入 [`http://127.0.0.1:8888/v1/chain/get_chain_info`](http://127.0.0.1:8888/v1/chain/get_chain_info)，来查看链的信息。

![chain_info](https://user-images.githubusercontent.com/1866848/59199390-683c1980-8bc8-11e9-8988-38b74e4d1a25.jpeg)

除此之外，也可以在bash终端命令行状态下，输入
`curl http://127.0.0.1:8888/v1/chain/get_chain_info`来查看链的信息。

![chain_info](https://user-images.githubusercontent.com/1866848/59199937-b6055180-8bc9-11e9-86a6-3a36d37265ef.png)

## 三、节点信息

### 开发环境（单链）

> httpEndpoint: "http://127.0.0.1:8888",  
httpEndpointHistory: "http://127.0.0.1:3000",  
chainId: "80a5d6aa3e0c2e2052c3df1cc6b591b90b8307fb102bd174805e06c8b8b16ec1",
  
### 测试网环境（多链）

**主链**
> httpEndpoint:"http://ultrain.natapp1.cc",  
 httpEndpointHistory:"http://ultrain-history.natapp1.cc",  
 chainId:"1f1155433d9097e0f67de63a48369916da91f19cb1feff6ba8eca2e5d978a2b2",

**侧链11**
> httpEndpoint:"http://pioneer.natapp1.cc",  
 httpEndpointHistory:"http://pioneer-history.natapp1.cc",  
 chainId:"20c35b993c10b5ea1007014857bb2b8832fb8ae22e9dcfdc61dacf336af4450f",

**侧链12**
> httpEndpoint:"http://power.natapp1.cc",  
 httpEndpointHistory:"http://power-history.natapp1.cc",  
 chainId:"0120d06d4a73b60357a5ed24a9145c967308738d70397c25eeedcbb736166ccf",

### 主网环境（多链）

**主链**
> httpEndpoint:"https://ultrain.services",  
 httpEndpointHistory:"https://history.ultrain.services",  
 chainId:"99b1cef2acdf6c4bcbce64c6490a999b819c236b19e3cd7cd2c3accc71da30ef",

**侧链poineer**
> httpEndpoint:"https://pioneer.ultrain.services",  
 httpEndpointHistory:"https://history-pioneer.ultrain.services",  
 chainId:"13c654dcffbed7b6d615aa92b75ebf1a3049ff74ffe73fdeafb9113be6b6fe22",

**侧链unitopia**
> httpEndpoint:"https://unitopia.ultrain.services",  
 httpEndpointHistory:"https://history-unitopia.ultrain.services",  
 chainId:"7c3040786b0d1de5af5bdba73800acb1767fbdea402da0613ba8601f3a1a2acb",

**侧链new-retail**
> httpEndpoint:"https://new-retail.ultrain.services",  
 httpEndpointHistory:"https://history-new-retail.ultrain.services",  
 chainId:"23b412f2ab81a33c1a3aabfa550984475accdd1d2906c26d77cabb17f53d24ac",

### 测试账户

本地环境为开发者默认创建了八个测试账号，这些账号分别拥有1000.0000UGAS和拥有无限的资源使用权。

| 账户名称 | 公钥                                                  | 私钥                                                |
| -------- | ----------------------------------------------------- | --------------------------------------------------- |
| alice    | UTR7zsqi7QUAjTAdyynd6DVe8uv4K8gCTRHnAoMN9w9CA1xLCTDVv | 5J9bWm2ThenDm3tjvmUgHtWCVMUdjRR1pxnRtnJjvKA4b2ut5WK |
| jerry    | UTR8Mz4buVKZiUXVxAhr4Bm4qM3V7bPJLWwqQ1eN4BxxcFJ3LosY9 | 5JFz7EbcsCNHrDLuf9VpHtnLdepL4CcAEXu7AtSUYfcoiszursr |
| tom      | UTR5p48ox7N8NfR8YR9H8ojgR3ewV3gCk6Z16BrUEGFSgonKUs1KR | 5KXYYEWSFRWfNVrMPaVcxiRTjD9PzHjBSzxhA9MeQKHPMxWP8kU |
| bob      | UTR5VE6Dgy9FUmd1mFotXwF88HkQN1KysCWLPqpVnDMjRvGRi1YrM | 5JoQtsKQuH8hC9MyvfJAqo6qmKLm8ePYNucs7tPu2YxG12trzBt |
| jack     | UTR6rBwNTWJSNMYu4ZLgEigyV5gM8hHiNinqejXT1dNGZa5xsbpCB | 5JC2uWa7Pba5V8Qmn1pQPWKDPgwmRSYeZzAxK48jje6GP5iMqmM |
| tony     | UTR6oPKRu3MyNhNN9YdXgxfGMTM2j2EhBAqYaQALmHZ7kcQmD9uE4 | 5KbHvFfDXovPvo2ACNd23yAE9kJF7Mxaws7srp6VapjMr7TrHZB |
| john     | UTR5CGbtir6mr6AbPfow6rgYYEo3WnMEctNL3mMWehXbDwvomqHQG | 5JxipaPk9qvM4xZ4ggRBGbntXatnwmGQ1ZHMaF9RSPLhyg17ALW |
| ben      | UTR6ujHgxt2hUz7BfvJz6epfvWzhXEp1ChVKEFZxf1Ld5ea83WE6V | 5JbedY3jGfNK7HcLXcqGqSYrmX2n8wQWqZAuq6K7Gcf4Dj62UfL |


## 四、浏览器

Ultrain平台环境可以分为线下开发环境、线上测试网环境与线上主网环境。其中，线上主网环境需要购买资源套餐后方可使用，
线上测试网环境可到测试网账户充值后使用，线下开发环境是指在本地自行构建的网络共识环境。以下篇幅重点介绍线上开发环境
需要使用浏览器进行的相关操作。

### 创建账号

创建账号有多种方式，可以使用[`测试网浏览器`](https://testnet-explorer.ultrain.io/ultrainio/account-recharge)或[Cona](https://developer.ultrain.io/tutorial/cona_introduce)
直接申请创建，也可以使用u3.js在代码中创建，前提是有一个账号作为creator，相关操作请参见u3.js
中账号创建的部分文档。

### 同步账号

账号需要同步到某一条侧链上才能进行相关的业务操作，如部署合约、发行代币等。账号同步可以使用[Cona](https://developer.ultrain.io/tutorial/cona_introduce)
直接进行账号同步或使用u3.js在代码中同步，相关操作请参见u3.js中账号同步的部分文档

### 资源套餐的购买

在部署合约前，需要为账号购买一定的资源套餐,你可以通过[测试网浏览器](https://testnet-explorer.ultrain.io/ultrainio/account-recharge)提供的相关资源套餐进行购买。
注意，资源套餐过期后，合约将自动回收，意味着合约中的数据将会被删除，所以如果
希望合约一直可用，必须在资源套餐过期前，对套餐的过期时间进行延长。购买或延长资源套餐需要消耗UGAS，除了自己为自己购买外，
也可以使用另一个账号进行购买，即可以为他人进行购买相关的资源套餐。前提是签名账号（即付费账号）必须要有足够的
UGAS。除了使用测试网浏览器进行资源套餐的购买之外，也可以通过[`开发者网站`](https://developer.ultrain.io/resources)上选择合适的资源套餐并进行购买
