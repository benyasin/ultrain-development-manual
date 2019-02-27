超脑链使用类Javascript的语言来编写智能合约，这个类Javascript的语言以Typescript为原型，通过扩展的数据类型标志符，来达到强类型语言的编程语法.

#### 系统内置的方法

function NAME\(str: string\): u64 : 方法\*\*NAME\(\)\*\*用来将一个string转成一个account\_name类型.`str`的字符长度不超过12个字符, 内容只能包括以下字符\(不能以`.`结尾\):`.012345abcdefghijklmnopqrstuvwxyz`

function RNAME\(account: account\_name\): string : 方法\*\*RNAME\(\)**用来将一个account\_name类型转为string类型, 它是**NAME\(\)\*\*方法的反向方法.

function ACTION\(str: string\): Action : 方法\*\*ACTION\(\)\*\*将一个string类型转为Action类型.`str`的长度不超过21个字符, 内容只能包括以下字符\(不能包含`.`\):`._0-9a-zA-Z`. Action类封装了action相关的信息.

Action.sender : 当前transaction的发起者, account\_name类型.

Action.receiver : 当前transaction的接收者, 即合约帐户, account\_name类型.

Block.number : head block的块高。

Block.id : head block的id，sha256的hash值。

Block.timestamp : head block的时间戳，从EPOCH开始的秒数。

#### 编写第一个合约Hello world

```
import { NAME, RNAME } from "ultrain-ts-lib/src/account";
import { Log } from "ultrain-ts-lib/src/log";
import { Contract } from "ultrain-ts-lib/lib/contract";

class HelloWorld extends Contract {

    @action
    hi(name: account_name, age: u32, msg: string): void {
        Log.s("hi: name = ").s(RNAME(name)).s(" age = ").i(age, 10).s(" msg = ").s(msg).flush();
    }
}
```

我们以上代码做如下说明：

1. import: 用来引入其它文件中定义的类和方法，详细用法可参考
   [typescript](https://www.tutorialspoint.com/typescript/index.htm)
   的说明。
2. extends Contract: 合约都需要派生自Contract，而且一个项目中**只能有一个Contract**
3. @action: 申明一个合约方法。只有@action标志的方法，才能被调用。
4. Log: 打印Log。需要在config.ini文件中配置
   `contracts-console = true`
   才能打印到终端。

#### 在Action中Return信息

为了便于在调用方与节点中传递部分执行状态信息，引入Return模块.  
Return模块返回的数据会附加在http的response中， 调用方可以通过分析response得到Return的信息。  
需要强调的是，**Return的信息仅仅是在一个节点\(host\_url \)上预执行的结果，并非区块链网络共识的结果。也就是说, Return返回的结果, 并不是最终交易执行的结果.**  
**Return的信息只供参考，它可能与区块链网络共识结果不一致。**

要Return信息，可以在action调用中，通过`Return`,`ReturnArray`方法来完成。Return信息有以下需要注意的点：

> NOTICE

1. Return的message是有长度限制的，默认的message长度为128个character。（int型数据会转成对应的string）。如果是在侧链中使用，可以在config.ini文件中配置
   `contract-return-string-length`
   来扩展长度限制。
2. 只支持Return基本数据类型int和string， 以及int\[\]和string\[\]。
3. 可以调用Return或ReturnArray多次，信息将被concat。
4. 超出长度限制的信息，会直接丢弃，不会抛出异常。

###### Return信息的示例

```
class HelloContract extends Contract{
    @action
    on_hi(name: u64, age: u32, msg: string): void {
        Return<string>("call hi() succeed.");
        ReturnArray<u8>([1,2,3]);
    }
}
```

执行正常的情况下，Return的结果是`call hi() succeed.123`

#### 资产查询和转移

在合约中，可以查询一个帐号在ultrainio.token合约中的资产，即ultrain平台资产。查询资产使用`Asset.balanceOf(who: account_name): Asset`方法。 转移ultrain平台资产，可以使用`Asset.transfer(from: account_name, to: account_name, val: Asset, memo: string): void`方法。

使用详情请参考[示例balance](https://github.com/ultrain-os/ultrain-ts-lib/blob/master/example/balance/balance.ts)。

```
import "allocator/arena";
import { Contract } from "ultrain-ts-lib/src/contract";
import { Asset } from "ultrain-ts-lib/src/asset";
import { ultrain_assert } from "ultrain-ts-lib/src/utils";
class BalanceContract extends Contract {

    @action
    transfer(from: account_name, to: account_name, bet: Asset): void {

        let balance = Asset.balanceOf(from);
        ultrain_assert(balance.gte(bet), "your balance is not enough.");

        balance.prints("banalce from: ");

        Asset.transfer(from, to, bet, "this is a transfer test");
    }
}
```

> NOTICE 使用Asset.transfer命令转移资产时，需要保证`from`的权限已经授权给了`utrio.code`，在使用命令行的情况下，可以通过以下命令来授权:
>
> `clutrain set account permission $from active '{"threshold": 1, "keys":[{"key":"$PubKey_of_from", "wieght": 1}], "accounts": [{"permission": {"actor": "$from", "permission": "utrio.code"}, "weight": 1]}' owner -p $from `
>
> `$from`是需要授权的帐号。



