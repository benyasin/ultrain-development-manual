如前文所述，超脑链为开发者提供了一整套开发工具与教程。详细的工具使用与文档教程请浏览开发者网站。

关于工具与文档的配合使用，我们以一个完整DApp的开发为例来讲解，通常分为几下几个步骤：
>1.	使用Longclaw或基于docker的命令行中构建本地开发环境；
2.	使用Robin framework初始化DApp项目目录结构（选择合适的UI模板）；
3.	使用TypeScript编写智能合约；
4.	使用Robin framework完成智能合约的编译与部署；
5.	使用U3.js与链交互，进行业务功能的测试；
6.	[可选]某些桌面应用，在业务代码中集成浏览器插件Cona完成转账或授权等操作；


## 一、Robin
<img src='https://user-images.githubusercontent.com/44561751/59588824-12f98e00-911b-11e9-94fb-5b03a67fd64d.png' style="zoom:50%">  

当你有了可用的开发环境后，接下来即可使用Robin framework去创建一个Dapp了。

### Robin的介绍
这是一款基于超脑链，能够快速进行智能合约开发、全局命令行式的开发测试集成框架。  
Robin框架还提供如下服务：
>1、	一键式合约初始化、编译与部署;  
2、	自动化合约测试与开发;  
3、	友好的代码审查与错误提示；  
4、	大量的合约模板与示例参考；  
5、	脚本式与可配置化部署流程；  
6、	交互式合约日志控制台输出；

### Robin的使用步骤

#### Robin的安装
	sudo npm install -g robin-framwork

#### Robin的使用要求
>NodeJS8.0+  
Linux、MacOS、Windows

#### 创建工程
执行`robin`或`robin -h`来查看所以的robin的子命令  
要启动项目，首先需要创建一个新的空目录，然后进入改目录：
> mkdir testing  
> cd testing

然后初始化一个项目。使用`-c`或`-contract`来指定名称。此时，你有多个模版可以选择，默认的是
纯合约项目，其余的是带界面的DAPP框架。
> robin init

![image](https://user-images.githubusercontent.com/44561751/59589432-7e902b00-911c-11e9-862e-f1e62cd35483.png)

** 合约项目目录结构如下：**
```
    |--build        // built WebAssembly targets
    |--contract     // contract source files
    |--migrations   // assign built files location and account who will deploy the contract
    |--templates    // some contract templates that will guide you
    |--test         // test files 
    |--config.js    // configuration
    ...
```
其中`config.js`中指定了上链交互的配置节点信息，`migrate.js`中声明了合约的`owner`账号。

#### 语法检查
在`robin-lint`的帮助下，借助定制的`tslint`项目，您将找到错误和警告，然后快速修复它们。 只需进入项目的根目录并执行：  
	
	robin lint
#### 编译合约
依赖于 ultrascript，合约源文件将会被编译为WebAsssembly目标文件: .abi, .wast, *.wasm. 只需进入项目的根目录并执行：
	
	robin build
#### 部署合约
更新配置文件 config.js 和 migrate.js, 确保你已正确连接上一个超脑节点. 如果你正在使用 longclaw 初始化的
本地环境，那么使用默认配置即可。如果您的 只需进入项目的根目录并执行：  
	
	robin deploy
#### 测试合约
参考测试目录下 *.spec.js 文件, 编写测试用例来覆盖你的合约中的所有用例场景. Robin提供给你一些测试工具类，
比如 mocha, chai, u3.js and u3-utils , 尤其是用在处理异步测试. 只需进入项目的根目录并执行：
	
	robin test
### 集成UI
如果你想将一个合约项目升级为带界面的DAPP项目, 使用UI子命令。你有多个框架可以选择，它们分别是 vue-boilerplate、
react-boilerplate 和 react-native-boilerplate. 只需进入项目的根目录并执行：
	
	robin ui


## 二、U3.js

<img src='https://user-images.githubusercontent.com/44561751/59590088-ded39c80-911d-11e9-9023-eb459de38319.png' style="zoom:50%"> 

U3.js主要的任务是Javascript封装的负责与链交互的通用库，具体的介绍如下：

### 应用环境
> 浏览器（ES6）或NodeJS
> 如果你想集成u3.js到react native环境中，又一个可行的方法，借助rn-nodeify实现，参考示例[U3RNDemo](https://github.com/benyasin/U3RNDemo)

### 使用方法
#### 在浏览器中使用U3
 请参考以下用法：

```
<!DOCTYPE html>
    <html lang="en">
        <head>
            <meta charset="UTF-8">
            <title>test</title>
            <script src="../dist/u3.js"></script>
            <script>
              let func = async () => {
                  let u3 = U3.createU3({
                    httpEndpoint: "http://127.0.0.1:8888",
                    httpEndpointHistory: "http://127.0.0.1:3000"
                  });
          
                  await u3.getChainInfo();
          
                  const keyProvider =
"5JbedY3jGfNK7HcLXcqGqSYrmX2n8wQWqZAuq6K7Gcf4Dj62UfL";
                  const c = await u3.contract("utrio.token");
                  await c.transfer("ben", "bob", "1.0000 UGAS", "", { keyProvider });
                };
                func();
            </script>
        </head>
        <body>
        </body>
    </html>

```

#### 在NodeJS环境中使用U3
请参照以下用法：
**安装u3**  
`npm install u3.js` 或`yarn add u3.js`

**初始化**  
```
const { createU3 } = require('u3.js');
let config = {
    httpEndpoint: "http://127.0.0.1:8888",
    httpEndpointHistory: "http://127.0.0.1:3000",
    chainId: "baf8bb9d3636379e3cd6779d2a72e693494670f1040d45154bb61dc8852c8971",
    broadcast: true,
    sign: true,
    logger: {
      directory: "../../logs", // daily rotate file directory
      level: "info", // error->warn->info->verbose->debug->silly
      console: true, // print to console
      file: false // append to file
    },
    symbol: "UGAS",
    //keyProvider:[],
    //expireInSeconds:60
}
let u3 = createU3(config);

u3.getChainInfo((err, info) => {
  if (err) {throw err;}
  console.log(info);
});
```

**本地环境运行**  
请参照[环境篇]的MacOS或Linux的环境搭建章节

**配置**
+ 全局配置
	+ httpEndpoint string - 链实时API的http或https地址.如果是在浏览器环境中使用u3，请注意配置相同的域名.
	+ httpEndpointHistory string - 链历史API的http或https地址.如果是在浏览器环境中使用u3，请注意配置相同的域名.
	+ chainId 链唯一的ID. 链ID可以通过 [httpEndpoint]/v1/chain/get_chain_info获得.
	+ keyProvider [array|string|function] - 提供私钥用来签名交易. 提供用于签名事务的私钥。如果提供了多个私钥，不能确定使用哪个私钥，可以使用调用get_required_keysAPI 获取要使用签名的密钥。如果是函数，那么每一个交易都将会使用该函数。如果这里不提供keyProvider,那么它可能会Options配置项提供在每一个action或每一个transaction中
	+ expireInSeconds number - 事务到期前的秒数，时间基于nodultrain的时间.
	+ broadcast [boolean=true] - 默认是true。使用true将交易发布到区块链，使用false将获取签名的事务.
	+ sign [boolean=true] - 默认是true。使用私钥签名交易。保留未签名的交易避免了提供私钥的需要.
	+ logger - 默认日志配置下
	
```
	logger: {
		directory: "../../logs", // 每天日志的目录
		level: "info", // error->warn->info->verbose->debug->silly
		console: true, // 打印到控制台
		file: false // 输出到文件
	}
```

+ Options配置项
 Options可以在方法参数之后添加`Authorization`应用于单独的`action`比如：
 ```
 options = {
   authorization: 'alice@active',
   broadcast: true,
   sign: true
 }
 
 u3.transfer('alice', 'bob', '1.0000 UGAS', '', options)
 ```  
	+ authorization [array|auth] - 指明账号和权限，典型地应用于多重签名的配置中. Authorization必须是一个字符串格式，形如account@permission。
	+ broadcast [boolean=true] - 默认是true。使用true将交易发布到区块链，使用false将获取签名的事务。
	+ sign [boolean=true] - 默认是true。使用私钥签名交易。保留未签名的交易避免了提供私钥的需要。
	+ keyProvider [array|string|function] - 就像global配置项中的keyProvider一样，这里的配置可以以覆盖全局配置的形式为每一个action或每一个transaction提供单独的私钥。		
```
await u3.anyAction('args', {keyProvider})
await u3.transaction(tr => { tr.anyAction() }, {keyProvider})
```
	

**创建账号**  
创建账号需要花费creator账号的一些代币，为新账号抵押部分RAM和带宽
```
const u3 = createU3(config);
const name = 'abcd1';//普通账号需要满足规则：必须为12345abcdefghijklmnopqrstuvwxyz中的5-12位，且必须同时包含数字与字母
let params = {
   creator: 'ben',
   name: name,
   owner: pubkey,
   active: pubkey,
   updateable: 1,//可选，账号是否可以更新（默认可以更新合约）
};
await u3.createUser(params);
```
主网与测试网为主侧链机制的多链架构，创建账号默认会先在主链上生成，然后需要同步到对应的侧链上，
这一过程需要账号自身提供授权主动同步。  

以下示例为账号tester1主动将自身从主链同步到pioneer侧链上的过程, 注意config的节点配置需提供
主链配置信息。
```
empoweruser(user,chain_name,owner_pk,active_pk,updateable)

const u3 = createU3(config);
const c = await u3.contract("ultrainio");
await c.empoweruser({
  user: 'tester1',
  chain_name: 'pioneer', // pioneer is one of the sidechain name
  owner_pk: 'UTR...',    // publicKey of owner permission
  active_pk: 'UTR...',   // publicKey of active permission 
  updateable: 1
})
```

**转账（UGAS**  
转账方法使用非常频繁，UGAS的转账需要调用系统合约utrio.token。  
+ transfer(from,to,content,memo) 其中content这个字段必须严格为金额与符号拼接的字符串, 如 ‘1.0000 UGAS’  

```
const u3 = createU3(config);
const c = await u3.contract('utrio.token')

// 使用位置参数
const result = await c.transfer('ben', 'bob', '1.2000 UGAS', '')
// 使用名称参数
const result = await c.transfer({from: 'bob', to: 'ben', quantity: '1.3000 UGAS', memo: ''})
```
+ 如果要确认这笔转账正确被执行了，可以遵循以下步骤:

```
// first check whether the transaction was failed
if (!result || result.processed.receipt.status !== "executed") {
console.log("the transaction was failed");
return;
}
// then check whether the transaction was irreversible when it was not expired
let timeout = new Date(result.transaction.transaction.expiration + "Z") - new Date();
await U3Utils.test.waitUntil(async () => {
let tx = await u3.getTxByTxId(result.transaction_id);
return tx && tx.irreversible;
}, timeout, 1000);
```

**签名**  
使用 { sign: false, broadcast: false } 创建一个u3实例并且做一些action, 然后将未签名的交易发送到钱包中。  
```
  const u3_offline = createU3({ sign: false, broadcast: false });
  const c = u3_offline.contract('utrio.token');
  let unsigned_transaction = await c.transfer('ultrainio', 'ben', '1 UGAS', 'uu');
```
在钱包中你可以提供私钥或助记词来签名，并将签名后的交易发送到链上。
```
  const u3_online = createU3();
  let signature = await u3_online.sign(unsigned_transaction, privateKeyOrMnemonic, chainId);
  if (signature) {
     let signedTransaction = Object.assign({}, unsigned_transaction.transaction, { signatures: [signature] });
     let processedTransaction = await u3_online.pushTx(signedTransaction);
  }
```

**资源**  
调用合约只会消耗合约Owner的资源，如果你想部署一个合约，请先购买一些资源。  
主网环境下,请至[`开发者网站`](https://developer.ultrain.io/resources)上选择合适的资源套餐并进行购买  
测试网环境下,请至[`测试网浏览器`](https://testnet-explorer.ultrain.io/ultrainio/account-recharge)自行进行账号充值与资源购买。
```
resourcelease(payer,receiver,slot,days,location)
location is the chain name you to use your resource
const u3 = createU3(config);
const c = await u3.contract('ultrainio')

await c.resourcelease('ben', 'bob', 1, 10, "ultrainio");// 1 slot for 10 days on the side chain named ultrainio
```
通过以下方法查询资源详情:
```
const account = await u3.getAccountInfo({ account_name: 'abcdefg12345' });
console.log(account.chain_resource[0].lease_num)
```

**合约**  
+ 部署合约  
部署合约需要提供包含目标文件为 .abi,.wast,*.wasm 的三个文件的文件夹。
	+ deploy(contracts_files_path, deploy_account) 第一个参数为合约目标文件的绝对路径，第二个合约部署者账号。  
```
const u3 = createU3(config);
await u3.deploy(path.resolve(__dirname, '../contracts/token/token'), 'bob');
```

+ 调用合约  
```
const u3 = createU3(config);
const c = await u3.contract('ben');
await c.transfer('bob', 'ben', '1.0000 UGAS','');
```  
或者可以像这样调用   
> await u3.contract('ben').then(sm => sm.transfer('bob', 'ben', '1.0000 UGAS',''))  

	一笔交易中也可以包含多个合约中的多个action
```
await u3.transaction(['ben', 'bob'], ({sm1, sm2}) => {
   sm1.myaction(..)
   sm2.myaction(..)
})
```

**发行代币**   
智能合约中需实现UIP06或UIP09的接口，具体例子请参照[项目模板](https://github.com/ultrain-os/robin-template)
```
const u3 = createU3(config);
const account = 'bob';
await u3.transaction(account, token => {
    token.create(account, '10000000.0000 DDD');
    token.issue(account, '10000000.0000 DDD', 'issue');
});

const balance = await u3.getCurrencyBalance(account, account, 'DDD')
console.log('currency balance', balance)
```

**事件**
Ultrain提供了一个事件注册监听机制用来解决异步场景下业务需求。客户端首先订阅一个事件，
提供一个用来接收消息的地址，当合约中的某个方法触发时，该地址会收到来自链的推送消息。  
+ 订阅/取消订阅
	+ registerEvent(deployer, listen_url)
	+ unregisterEvent(deployer, listen_url)  
	deployer : 合约的部署者账号  
	listen_url : 接收消息的地址  
	注意: 如果你是在本地docker环境中使用改机制，请确认接收地址是一个可以从docker访问到的本地宿主地址。
```
const u3 = createU3(config);
const subscribe = await u3.registerEvent('ben', 'http://192.168.1.5:3002');
```  
	或  
```
const unsubscribe = await u3.unregisterEvent('ben', 'http://192.168.1.5:3002');
```  
	监听示例如下：  
```
const { createU3, listener } = require('u3.js');
listener(function(data) {
   // do callback logic
   console.log(data);
});
U3Utils.test.wait(2000);
//must call listener function before emit event
const contract = await u3.contract(account);
contract.hi('ben', 30, 'It is a test', { authorization: [`ben@active`] });
```
 
## 三、Cona  

<img src='https://user-images.githubusercontent.com/44561751/59646092-dc1d8980-91a7-11e9-9e62-445790769098.png' style="zoom:50%">

Cona是一款基于浏览器插件的超脑链轻钱包，涵盖转账、收款、账号同步及授权认证等功能，可以让你在浏览器环境
中运行超脑链的DApp。  
### Cona主要特性  
>导入账户  
创建测试网账户  
账户详情  
查看交易历史  
查看代币列表  
发起转账交易  
账户同步到侧链  
账户设置-导出私钥助记词  
设置-切换网络、中英文及密码修改  

### Cona否安装  
Cona扩展插件会在浏览器注入window.Cona对象，检测window.Cona如果存在则表示用户有安装。  
>Cona在线安装地址为 https://chrome.google.com/webstore/detail/cona/joopmnkobcdaojgcmohnjhloldhfgfgk  
Cona离线下载地址为 https://ultrain-cona.oss-cn-hangzhou.aliyuncs.com/cona.crx
  
```
window.addEventListener('load', function () {
  if (typeof window.Cona !== 'undefined') {
    console.log('Cona is enabled')
  } else {
    console.log('Cona is not installed')
  }
})
```  
  
### Cona.version  
查看Cona的版本号  
```
window.addEventListener('load', function () {
  if (typeof window.Cona !== 'undefined') {
    console.log(window.Cona.version);
  }
})
```  
### 创建钱包  
安装成功后，在chrome浏览器右上角点击Cona图标，打开Cona插件。主界面有主网测试网切换、中英文切换。按提示填写表单即可创建一个钱包。（钱包创建后，并没有账户，账户需要导入）  
<img src='https://developer.ultrain.io/upload/images/img20190605173636.png' style="zoom:50%">

### 部分功能入口及页面  

#### 账户详情  
​账户详情页面，可以看到账户的UGAS余额、交易历史，以及代币查看、转账交易、账户同步的操作入口。  
<img src='https://developer.ultrain.io/upload/images/img20190605173723.png' style="zoom:50%">

#### 查看代币列表  

<img src='https://developer.ultrain.io/upload/images/img20190605173744.png' style="zoom:50%">
<img src='https://developer.ultrain.io/upload/images/img20190605173807.png' style="zoom:50%">

#### 账户设置——导出私钥  
​账户设置，主要功能有导出私钥、导出助记词等  
<img src='https://developer.ultrain.io/upload/images/img20190605173829.png' style="zoom:50%">
<img src='https://developer.ultrain.io/upload/images/img20190605174001.png' style="zoom:50%">

#### 设置  
​设置页面可以进行网络切换、语言切换，以及钱包修改密码  
<img src='https://developer.ultrain.io/upload/images/img20190605174023.png' style="zoom:50%">

### Cona.send(params)  
发起交易请求  
```
 const to = 'utest1';
    const contract = 'utrio.token';
    const quantity = 10;
    const symbol = 'UGAS';
    const memo = 'transfer 10.0000 UGAS';
    window.Cona.send({ to, contract, quantity, symbol, memo }).then((trx) => {
      // trx为链上返回的交易详情，需要通过u3轮询交易来确认交易结果
      console.log(trx);
    }).catch((e) => {
      // 处理异常
      console.log(e);
});
```  
params参数介绍如下所示：
>to: 接收账户，类型为字符串  
contract: 交易的合约名称，类型为字符串  
quantity: 转账金额，精度请按代币约定填写，如果超出代币的精度则将会被四舍五入，例如UGAS的精度是0.0001，如果传入10.66666，则会被四舍五入为10.6667  
symbol: 代币单位  
memo: 交易描述文字，可以不填写  
返回值：方法是一个异步方法，返回一个promise对象，可以在then中获取到链上返回的交易信息。在catch中捕获异常。  
注意，为确保send方法能够被正常调起，请首先安装Cona插件，选择合适的网络进行登录，并导入至少一个钱包。  

## 四、UltrainOne  
为整合和规范管理Ultrain全球社区，Ultrain官方团队开发了Ultrain官方App——UltrainOne。
同时支持iOS和Android系统下载使用，注重用户体验，操作方便，界面简洁美观，并开放多种使用功能，
可供用户轻松使用。  

目前用户可使用此App在移动端进行创建Ultrain测试网和主网的钱包、导入钱包、导出公私钥、多钱包管理等功能操作，
简单易用，并可以使用助记词、私钥两种钱包备份方案，防丢防盗。  

除一些钱包功能外，UltrainOne APP还具有如下功能
> ** 全新积分系统**：（每100积分价值1UGAS），除了日常打卡获得积分外，用户可参与不同时期的官方活动获得积分奖励。  
> ** 商城**：Ultrain周边商品可使用积分在商城模块中进行兑换，如Supernova超星小象等。  
> ** 应用商店入口**：用户可以在UltrainOne APP内查看区块链相关的APP和DApp链接。  
> ** UGAS查询**：用户除了可以在App上使用移动端钱包外，还可以直接查询ETH钱包内UGAS的数量。  
> ...  

### UltrainOne下载地址：

![APP](https://ultrain.io/public/images/app-zh.png)
