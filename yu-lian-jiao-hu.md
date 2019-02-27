U3.js是用Javascript封装的负责与链交互的通用库。而Robin framework引用了U3.js，借助它的接口实现了合约的deploy上链。

## 应用环境

浏览器（ES6）或 NodeJS

如果你想集成u3.js到react native环境中，有一个可行的方法,借助rn-nodeify实现，参考示例[U3RNDemo](https://github.com/benyasin/U3RNDemo)

## 使用方法

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

* #### 安装u3

`npm install u3.js`或`yarn add u3.js`

* #### 初始化

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



