## 五、与链交互

U3.js是用Javascript封装的负责与链交互的通用库。而Robin framework引用了U3.js，借助它的接口实现了合约的deploy上链。

#### 应用环境

浏览器（ES6）或 NodeJS

如果你想集成u3.js到react native环境中，有一个可行的方法,借助rn-nodeify实现，参考示例[U3RNDemo](https://github.com/benyasin/U3RNDemo)

#### 使用方法

一、如果是在浏览器中使用u3，请参考以下用法：

```
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>test</title>
<script src="../dist/u3.js"></script>
<script>        
     let u3 = U3.createU3(
        httpEndpoint: 'http://127.0.0.1:8888', 
        httpEndpoint_history: 'http://127.0.0.1:3000',
        broadcast: true,
        debug: false,
        sign: true,
        logger: {
          log: console.log, 
          error: console.error, 
          debug: console.log 
        },
        chainId:'0eaaff4003d4e08a541332c62827c0ac5d96766c712316afe7ade6f99b8d70fe',
        symbol: 'UGAS'
     });
     u3.getChainInfo((err, info) =>
        if (err) {throw err;}
           console.log(info);
     });
</script>
</head>
<body>
</body>
</html>
```

二、 如果是在NodeJS环境中使用u3，请参照以下用法：

* ##### 安装u3

`npm install u3.js`或`yarn add u3.js`

* ##### 初始化

```
const { createU3 } = require('u3.js/src');
let config = {
  httpEndpoint: 'http://127.0.0.1:8888',
  httpEndpoint_history: 'http://127.0.0.1:3000',
  chainId: '0eaaff4003d4e08a541332c62827c0ac5d96766c712316afe7ade6f99b8d70fe',
  keyProvider: ['PrivateKeys...'],
  broadcast: true,
  sign: true
}
let u3 = createU3(config);

u3.getChainInfo((err, info) =>{
  if (err) {throw err;}
  console.log(info);
});
```

#### 配置

##### 全局配置

* **httpEndpoint**
  string - 链实时API的http或https地址.如果是在浏览器环境中使用u3，请注意配置相同的域名.
* **httpEndpoint\_history**
  string - 链历史API的http或https地址.如果是在浏览器环境中使用u3，请注意配置相同的域名.
* **chainId**
  链唯一的ID. 链ID可以通过 \[httpEndpoint\]/v1/chain/get\_chain\_info获得.
* **keyProvider**
  \[array\|string\|function\] - 提供私钥用来签名交易. 提供用于签名事务的私钥。 如果提供了多个私钥，不能确定使用哪个私钥，可以使用调用get\_required\_keysAPI 获取要使用签名的密钥. 如果是函数，那么每一个交易都将会使用该函数. 如果这里不提供keyProvider,那么它可能会Options配置项提供在每一个action或每一个transaction中
* **expireInSeconds**
  number - 事务到期前的秒数，时间基于nodultrain的时间.
* **broadcast**
  \[boolean=true\] - 默认是true。使用true将交易发布到区块链，使用false将获取签名的事务.
* **verbose**
  \[boolean=false\] - 默认是false。详细日志记录.
* **debug**
  \[boolean=false\] - 默认是false。低级调试日志记录.
* **sign**
  \[boolean=true\] - 默认是true。使用私钥签名交易。保留未签名的交易避免了提供私钥的需要.
* **logger**
  * 默认日志配置.

```
logger: {
  log: config.verbose ? console.log : null,  // 如果值为null，则禁用日志
  error: config.verbose ? console.error : null,
}
```

##### Options配置项

Options可以在方法参数之后添加. Authorization应用于单独的actions.比如:

```
options = {
  authorization: 'alice@active',
  broadcast: true,
  sign: true
}
```

```
u3.transfer('alice', 'bob', '1.0000 UGAS', '', options)
```

* **authorization**
  \[array\|auth\] - 指明账号和权限，典型地应用于多重签名的配置中. Authorization必须是一个字符串格式，形如account@permission.
* **broadcast**
  \[boolean=true\] - 默认是true。使用true将交易发布到区块链，使用false将获取签名的事务.
* **sign**
  \[boolean=true\] - 默认是true。使用私钥签名交易。保留未签名的交易避免了提供私钥的需要.
* **keyProvider**
  \[array\|string\|function\] - 就像global配置项中的keyProvider一样，这里的配置可以以覆盖全局配置的形式为每一个action或每一个transaction提供单独的私钥.

```
await u3.anyAction('args', {keyProvider})
await u3.transaction(tr ={ 
    tr.anyAction()}, {keyProvider}
)
```

#### 创建账号

创建账号需要花费creator账号的一些代币，为新账号抵押部分RAM和带宽

```
const u3 = createU3(config);
const name = 'abcdefg12345';//普通账号需要满足规则：必须为12345abcdefghijklmnopqrstuvwxyz中的12位
let params = {
   creator: 'ben',
   name: name,
   owner: pubkey,
   active: pubkey,
   updateable: 1,//可选，账号是否可以更新（更新合约）
};
await u3.createUser(params);
```

#### 转账（UGAS）

转账方法使用非常频繁，UGAS的转账需要调用系统合约utrio.token.

```
const u3 = createU3(config);
const c = await u3.contract('utrio.token')

// 使用位置参数
await c.transfer('ben', 'bob', '1.2000 UGAS', '')
// 使用名称参数
await c.transfer({from: 'bob', to: 'ben', quantity: '1.3000 UGAS', memo: ''})
```

#### 签名

使用`{ sign: false, broadcast: false }`创建一个u3实例并且做一些action, 然后将未签名的交易发送到钱包中.

```
  const u3_offline = createU3({ sign: false, broadcast: false });
  const c = u3_offline.contract('utrio.token');
  let unsigned_transaction = await c.transfer('ultrainio', 'ben', '1 UGAS', 'uu');
```

在钱包中你可以提供私钥或助记词来签名，并将签名后的交易发送到链上.

```
  const u3_online = createU3();
  let signature = await u3_online.sign(unsigned_transaction, privateKeyOrMnemonic, chainId);
  if (signature) {
     let signedTransaction = Object.assign({}, unsigned_transaction.transaction, { signatures: [signature] });
     let processedTransaction = await u3_online.pushTx(signedTransaction);
  }
```

#### 资源

调用合约只会消耗合约Owner的资源，所以如果你想部署一个合约，请先购买一些资源.

* resourcelease\(payer,receiver,slot,days\)

```
const u3 = createU3(config);
const c = await u3.contract('ultrainio')

await c.resourcelease('ben', 'bob', 1, 10);// 1 slot for 10 days
```

通过以下方法查询资源详情.

```
const resource = await u3.queryResource('abcdefg12345');
console.log(resource)
```

#### 合约

##### 部署合约

部署合约需要提供包含目标文件为_.abi,_.wast,\*.wasm 的三个文件的文件夹.

* deploy\(contracts\_files\_path, deploy\_account\) 第一个参数为合约目标文件的绝对路径，第二个合约部署者账号.

```
  const u3 = createU3(config);
  await u3.deploy(path.resolve(__dirname, '../contracts/token/token'), 'bob');
```

##### 调用合约

```
const u3 = createU3(config);
const c = await u3.contract('ben');
await c.transfer('bob', 'ben', '1.0000 UGAS','');

//或者像这样调用
await u3.contract('ben').then(sm =>
    sm.transfer('bob', 'ben', '1.0000 UGAS','')
)

// 一笔交易也可以包含多个合约中的多个action
await u3.transaction(['ben', 'bob'], ({sm1, sm2}) =>{
   sm1.myaction(..)
   sm2.myaction(..)
})
```

##### 发行代币

```
const u3 = createU3(config);
const account = 'bob';
await u3.transaction(account, token =>{
    token.create(account, '10000000.0000 DDD');
    token.issue(account, '10000000.0000 DDD', 'issue');
});

const balance = await u3.getCurrencyBalance(account, account, 'DDD')
console.log('currency balance', balance)
```

#### 事件

Ultrain提供了一个事件注册监听机制用来解决异步场景下业务需求.客户端首先订阅一个事件，提供一个用来接收消息的地址， 当合约中的某个方法触发时，该地址会收到来自链的推送消息.

##### 订阅/取消订阅

* registerEvent\(deployer, listen\_url\)
* unregisterEvent\(deployer, listen\_url\)

**deployer**: 合约的部署者账号

**listen\_url**: 接收消息的地址

注意: 如果你是在本地docker环境中使用改机制，请确认接收地址是一个可以从docker访问到的本地宿主地址.

```
const u3 = createU3(config);
const subscribe = await u3.registerEvent('ben', 'http://192.168.1.5:3002');

//or
const unsubscribe = await u3.unregisterEvent('ben', 'http://192.168.1.5:3002');
```

##### 监听

    const { createU3, listener } = require('u3.js/src');
    listener(function(data) {
       // do callback logic
       console.log(data);
    });

    U3Utils.test.wait(2000);

    //must call listener function before emit event
    const contract = await u3.contract(account);
    contract.hi('ben', 30, 'It is a test', { authorization: [`ben@active`] });



