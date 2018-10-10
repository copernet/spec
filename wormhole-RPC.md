## 表一：数据查询

RPC |feature |
---|----|----
whc_getinfo | 获取wormhole节点的基础信息|
whc_getactivecrowd | 获取指定地址的活跃众筹|
whc_getallbalancesforaddress | 获取指定地址所有种类的token金额|
whc_getallbalancesforid | 获取wormhole系统中含有指定token的所有地址及金额信息|
whc_getbalance | 获取指定地址上指定token的金额信息|
whc_getbalanceshash | 获取本节点当前高度下指定token的状态哈希|
whc_getcrowdsale | 获取众筹token的详细信息|
whc_getcurrentconsensushash | 获取本节点当前高度下wormhole系统的状态哈希|
whc_getgrants | 获取指定管理token的增发，销毁信息|
whc_getpayload | 获取指定交易的wormhole载荷数据|
whc_getproperty | 获取指定token的信息|
whc_getsto | 获取指定空投交易的详细信息|
whc_gettransaction | 获取指定交易的wormhole协议信息|
whc_listblocktransactions | 获取指定区块中的wormhole交易列表|
whc_listpendingtransactions | 获取节点待确认的wormhole交易列表|
whc_listproperties | 列出wormhole系统中的所有token|
whc_listtransactions | 列出与节点钱包中的wormhole交易|
whc_getfrozenbalance | 获取指定地址指定token下的冻结资金信息 |
whc_getfrozenbalanceforid | 获取指定token下的全部地址冻结资金信息 |
whc_getfrozenbalanceforaddress | 获取指定地址下的全部种类token冻结信息 |

### whc_getinfo
解释：获取当前wormhole节点的基本信息

调用：`wormholed-cli whc_getinfo`

返回值：当前wormhole节点的基本信息

示例如下
```
wormholed-cli whc_getinfo
{
  "wormholeversion_int": 6000,
  "wormholeversion": "0.0.6",
  "bitcoincoreversion": "0.17.2",
  "block": 543198,
  "blocktime": 1534136847,
  "blocktransactions": 0,
  "totaltransactions": 1099,
  "alerts": [
  ]
}
```

返回值字段描述
* wormholeversion_int : wormhole 节点的版本
* wormholeversion ：wormhole 节点的版本
* bitcoincoreversion ：bitcoin-abc 的版本
* block ：节点最新的区块高度
* blocktime：节点最新的区块时间戳
* blocktransactions ：当前最新的区块中包含的wormhole交易个数
* totaltransactions ：当前高度下，块链中所有wormhole交易的个数
* alerts ：节点规则的警告信息

### whc_getactivecrowd
解释：获取指定地址的活跃众筹

调用：`wormholed-cli whc_getactivecrowd address`

参数：`address` : 指定查询地址

返回值

*   该地址存在活跃众筹时，返回活跃众筹token的信息
*   不存在活跃众筹，返回`{}`.
示例如下
```
wormholed-cli whc_getactivecrowd  qq893ghdg697e5t5anh5fqpxwxxhw3akyu9l7wej0q
{ 
 "propertyid": 168,  
  "name": "bcext",  
  "category": "bcext development",  
  "subcategory": "gcash cashwallet cashutil neutrino",  
  "data": "contribute to bitcoin-abc",  
  "url": "https://github.com/bcext",  
  "precision": 8,  
  "issuer": "bitcoincash:qq893ghdg697e5t5anh5fqpxwxxhw3akyu9l7wej0q",  
  "creationtxid": "1f2a39a5fce4e2ce877b611925ef4c6eedb805c1337edd8f1d4bab49e2fe2449",  
  "totaltokens": "100000000.12345678"
}
```

返回值字段描述

*   propertyid : tokenID
*   name : token名称
*   category : token类别
*   subcategory : token子类别
*   data : token信息
*   url : token的URL
*   precision : token精度
*   issuer : 发行者
*   creationtxid : 创建该token的交易ID
*   totaltokens : 总共发行的众筹token数量


### whc_getallbalancesforaddress
解释：获取指定地址所有类型token的非0余额信息；对于金额为0的token，不显示

调用：`wormholed-cli whc_getallbalancesforaddress address`

参数：`address` : 指定查询地址

返回值
    
*   存在金额非0的token，返回该token的金额信息；否则返回`{}`.

示例如下
```
wormholed-cli whc_getallbalancesforaddress qr3pzyxl33vdhga54rvh80s62pjznl9eyu9k7q9tmv
[
       {
         "propertyid": 1,        
         "balance": "14.00000000",
         "reserved": "0.00000000"
       },
       {
         "propertyid": 3,
         "balance": "500",
         "reserved": "0"
       }
]
```

返回值字段描述

*   propertyid : tokenID
*   balance : 可用余额
*   reserved : 冻结余额，当前版本不可用


### whc_getallbalancesforid
解释：获取当前块链中含有指定token的所有地址及该token的金额信息

调用：`wormholed-cli whc_getallbalancesforid property`

参数：`property` : tokenID

返回值

*   含有该token的地址列表

示例如下
```
wormholed-cli whc_getallbalancesforid 1
[
   {
       "address": "bitcoincash:qrpcu87d3y83jg6pjhxrk7ys2225rp9m25nypfvtvk",
       "balance": "0.00003000",
       "reserved": "0.00000000"
   },
   {
       "address": "bitcoincash:qzf6rh7z40963a9jhh8agmnexw486g205upjjhsfzl",
       "balance": "100.00000000",
       "reserved": "0.00000000"
   }
]
```

返回值字段描述

*   address : 地址
*   balance : 可用余额
*   reserved : 冻结余额，当前版本不可用


###  whc_getbalanceshash
描述：返回当前节点最高区块状态下，指定token在系统中存在的状态哈希

调用：`wormholed-cli whc_getbalanceshash 1`

参数：指定的tokenID

返回值：token在系统中存在的状态哈希

示例如下
```
wormholed-cli whc_getbalanceshash 1
{
  "block": 542347,
  "blockhash": "000000000000000000f6d63c8adeff1f4bc6fe3618d73f74813c05f08761060a",
  "propertyid": 1,
  "balanceshash": "f4a051549368b79409b25ad5c3dba4a9e8b0434996d88be8281969f204b35dee"
}
```

返回值字段描述

*   block : 当前节点的最高区块号
*   blockhash : 该区块的哈希
*   propertyid : tokenID
*   balanceshash : token在系统中存在的状态哈希


### whc_getbalance
描述：返回指定地址，指定token的金额信息

调用：`wormholed-cli whc_getbalance address propertyid`

参数

*   `address` ：指定的地址
*   `propertyid` ：tokenID

返回值：指定地址的指定token的金额信息
示例如下
```
wormholed-cli whc_getbalance qqmrktdkuj0qtu0dyef0h2xkn7u6stycuvk70k0ups 1
{
  "balance": "5.00000000",
  "reserved": "0.00000000"
}
```

返回值字段描述

*   balance : 可用余额
*   reserved : 冻结余额，当前版本不可用


### whc_getcrowdsale
描述：获取众筹token的信息

调用：`wormholed-cli whc_getcrowdsale propertyid (verbose)`

参数：
    
*   propertyid ：众筹token的ID
*   verbose ：true, 列出众筹参与的信息；false, 不列出众筹参与的信息

返回值：返回众筹token的信息

示例如下
```
wormholed-cli  whc_getcrowdsale  168 true
{
  "propertyid": 168,
  "name": "bcext",
  "active": true,
  "issuer": "bitcoincash:qq893ghdg697e5t5anh5fqpxwxxhw3akyu9l7wej0q",
  "propertyiddesired": 1,
  "precision": "8",
  "tokensperunit": "1000.11111111",
  "earlybonus": 10,
  "starttime": 1533266569,
  "deadline": 1534733523,
  "amountraised": "1.12345000",
  "tokensissued": "100000000.12345678",
  "addedissuertokens": "0.00000000",
  "participanttransactions": [
    {
      "txid": "ebdc302cddb8fad9e8e11a210af7c01e57b4cf3c883b1ff7d504b4a3791177da",
      "amountsent": "1.12345000",
      "participanttokens": "1393.76317766"
    }
  ]
}
```

返回值字段描述

*   propertyid ：tokenID
*   name ：token名称
*   active ：众筹是否处于活跃状态
*   issuer ：众筹的发行者
*   propertyiddesired ：募集的tokenID
*   precision ：众筹token的精度
*   tokensperunit ： 该众筹token的单价，即1WHC等于多少token。
*   earlybonus ：早鸟奖励
*   starttime ： 该众筹开始的时间
*   deadline ： 该众筹截止时间
*   amountraised ：已募集到的资金
*   tokensissued ： 众筹token发行的数量
*   addedissuertokens ：当众筹关闭时，未售完的众筹数量，这些数量的token 会计入发行者的账户地址
* participanttransactions ：众筹参与者的信息
          
    *   txid：参与众筹的交易ID
    *   amountsent：参与众筹的WHC金额
    *   participanttokens：购买到的众筹token数量

### whc_getcurrentconsensushash
描述：获取当前wormhole系统的状态哈希

调用：`wormholed-cli whc_getcurrentconsensushash`

返回值：系统状态的哈希

示例如下
```
wormholed-cli whc_getcurrentconsensushash
{
  "block": 542354,
  "blockhash": "00000000000000000104a3002e3ddf7ccc835751e09ef3335a22078b129c2c71",
  "consensushash": "95e4e7fa0d92f84b18ec45ad9b39fdafcc5200ba8175227d7d571864aa5948c9"
}
```

返回值字段描述

*   block : 当前节点的最高区块号
*   blockhash : 该区块的哈希
*   consensushash : 系统状态的哈希

### whc_getgrants
描述：返回管理token的增发或销毁信息

调用：`wormholed-cli whc_getgrants propertyid`

参数：tokenID

返回值：管理token的增发或销毁信息

示例如下
```
wormholed-cli whc_getgrants 166
{
  "propertyid": 166,
  "name": "blockchain extension development",
  "issuer": "bitcoincash:qpadl79yr4hh0fym4gw73mxp3rm4325knqe7fj6a9s",
  "creationtxid": "a82b29d69538268fb67df384b9be7128456724e6fa69e4eb387944cb78ed9d66",
  "totaltokens": "0.00000",
  "issuances": [
    {
        "txid":
        "grant":
    }
    {
        "txid":
        "revoke":
    }
  ]
}
```

返回值字段描述

*   propertyid ： tokenID
*   name ：token名称
*   issuer ：token发行者
*   creationtxid ：创建该笔token的交易ID
*   totaltokens ： 当前token的发行量
*   issuances ：token管理的信息
    * txid ：增发或销毁的交易ID
    * grant：增发的token数量
    * revoke：销毁的token数量


### whc_getpayload
描述：获取指定交易的wormhole载荷数据

调用：`wormholed-cli whc_getpayload txid`

参数：交易ID

返回值：指定交易中wormhole协议的数据

示例如下
```
wormholed-cli whc_getpayload a82b29d69538268fb67df384b9be7128456724e6fa69e4eb387944cb78ed9d66
{
  "payload": "00000036010005000000006263657874207265706f7369746f7279006763617368206361736877616c6c657420636173687574696c206e65757472696e6f00626c6f636b636861696e20657874656e73696f6e20646576656c6f706d656e740068747470733a2f2f6769746875622e636f6d2f626365787400666f72207468652066757475726500",
  "payloadsize": 136
}
```

返回值字段描述

*   payload ：载荷数据
*   payloadsize ：载荷数据的长度


### whc_getproperty
描述：获取指定token信息

调用：`wormholed-cli whc_getproperty propertyid`

参数：tokenID

返回值：该token的详细信息

示例如下
```
wormholed-cli whc_getproperty 168
{
  "propertyid": 168,
  "name": "bcext",
  "category": "bcext development",
  "subcategory": "gcash cashwallet cashutil neutrino",
  "data": "contribute to bitcoin-abc",
  "url": "https://github.com/bcext",
  "precision": 8,
  "issuer": "bitcoincash:qq893ghdg697e5t5anh5fqpxwxxhw3akyu9l7wej0q",
  "creationtxid": "1f2a39a5fce4e2ce877b611925ef4c6eedb805c1337edd8f1d4bab49e2fe2449",
  "fixedissuance": false,
  "managedissuance": false,
  "totaltokens": "100000000.12345678"
}
```

返回值字段描述

*   propertyid ：tokenID
*   name ：token名称
*   category : token类别
*   subcategory : token子类别
*   data : token信息
*   url : token的URL
*   precision : token精度
*   issuer : 发行者
*   fixedissuance : 是否属于固定属性token
*   managedissuance : 是否属于可管理token
*   creationtxid : 创建该token的交易ID
*   totaltokens : 总共发行的众筹token数量


### whc_getsto
描述：获取空投相关的信息

调用：`wormholed-cli whc_getsto txid recipientfilter`

参数

*   txid : 空投的交易ID
*   recipientfilter : 过滤器；指定地址时，只显示该接收的空投金额信息；当为`*`时，默认显示所有账户接收的空投金额信息。
返回值：空投的详细信息

示例如下
```
wormholed-cli whc_getsto 403ec9b6f8b142485ea514d52bc4c782f008021a261f637028a28e1a64681d1b
{
  "txid": "403ec9b6f8b142485ea514d52bc4c782f008021a261f637028a28e1a64681d1b",
  "fee": "268",
  "sendingaddress": "bchtest:qz04wg2jj75x34tge2v8w0l6r0repfcvcygv3t7sg5",
  "ismine": false,
  "version": 0,
  "type_int": 3,
  "type": "Send To Owners",
  "propertyid": 12,
  "precision": "1",
  "amount": "50.0",
  "totalstofee": "0",
  "recipients": [
  ],
  "valid": false,
  "invalidreason": "Unknown error",
  "blockhash": "000000000000037252bf77bba30e5599b20239eb8f9f68c8b18c238688a27f6b",
  "blocktime": 1531904498,
  "positioninblock": 999999,
  "block": 1247269,
  "confirmations": 3368
}
```

返回值字段描述

*   txid : 该空投交易ID
*   fee  : 该交易的BCH手续费
*   sendingaddress : 空投交易的发送者
*   ismine ：该发送地址是否属于节点钱包中的地址
*   version ：空投类型的版本号
*   type_int ：空投交易类型
*   type ： 空投交易类型
*   propertyid ：空投的tokenID
*   precision ：空投的token精度
*   amount ：空投的金额
*   totalstofee ： 这笔空投花费的手续费
*   valid ：是否有效的空投交易
*   invalidreason ： 无效的原因
*   blockhash ：该交易所在的区块哈希
*   blocktime ：该交易所在区块的时间戳
*   positioninblock ：该交易在区块中的索引
*   block ： 该交易所在的区块号
*   confirmations ：该交易被确认的次数
*   recipients ：这次空投接收者的信息
    *   address ：接收此次空投的地址
    *   amount ：接收的空投金额


### whc_gettransaction
描述：获取wormhole交易的信息

调用：`wormholed-cli whc_gettransaction txid`

参数：交易ID

返回值：如果为wormhole交易，则返回它的详细信息

示例如下
```
wormholed-cli whc_gettransaction a82b29d69538268fb67df384b9be7128456724e6fa69e4eb387944cb78ed9d66
{
  "txid": "a82b29d69538268fb67df384b9be7128456724e6fa69e4eb387944cb78ed9d66",
  "fee": "390",
  "sendingaddress": "bitcoincash:qpadl79yr4hh0fym4gw73mxp3rm4325knqe7fj6a9s",
  "ismine": false,
  "version": 0,
  "type_int": 54,
  "type": "Create Property - Manual",
  "propertyid": 166,
  "precision": "5",
  "ecosystem": "main",
  "category": "bcext repository",
  "subcategory": "gcash cashwallet cashutil neutrino",
  "propertyname": "blockchain extension development",
  "data": "for the future",
  "url": "https://github.com/bcext",
  "amount": "0.00000",
  "valid": true,
  "blockhash": "0000000000000000009179a89e5ef71fabac5404c61a11cc60a21c1d6caefb7e",
  "blocktime": 1533194387,
  "positioninblock": 1553,
  "block": 541634,
  "confirmations": 729
}
```

返回值字段描述

*   txid : 交易ID
*   fee  : 该交易的BCH手续费
*   sendingaddress : 交易的发送者
*   ismine ：该发送地址是否属于节点钱包中的地址
*   version ：wormhole类型的版本号
*   type_int ：wormhole的交易类型
*   wormholed-cli ： wormhole的交易类型
*   propertyid ：如为创建token类型的交易，标识tokenID
*   precision ：精度
*   ecosystem ：该token所在的生态体系
*   category ： 创建的token类别
*   amount ：创建的token数量
*   valid ： 该笔wormhole交易是否有效
*   blockhash ：该交易所在的区块哈希
*   blocktime ：该交易所在区块的时间戳
*   positioninblock ：该交易在区块中的索引
*   block ： 该交易所在的区块号
*   confirmations ：该交易被确认的次数


### whc_listblocktransactions
描述：返回指定块中所有wormhole交易

调用：`wormholed-cli whc_listblocktransactions blockHeight`

参数：块号

返回值：wormhole的交易哈希列表

示例如下
```
wormholed-cli whc_listblocktransactions 541634
[
  "599e5126759a98c977fce4056b14628f4e175d757d090ed11809c9f0474e6d55",
  "fa072ca373c6a38248207eab6a4e85933792628188f6ba6eb99a0fa719d8e808",
  "ec451d67689ed2d990652a013b1af87edce694f5675c11b5063c89889f5fa8ed",
  "548a10ca6c0b36bed39a78e4d47636fa16ccd3b9ba0dadb5882deb4933f83336",
  "72d266f60e6c7ddb64b5009b59bc263da4ee1be89257b1fd1625a3c674b23795",
  "a82b29d69538268fb67df384b9be7128456724e6fa69e4eb387944cb78ed9d66",
  "0956f48b0b097df6d3aa2d34acfe75a362a185fdfe32f90b5683f178558d5569"
]
```

### whc_listpendingtransactions
描述：查询节点交易池中相关地址未确认的wormhole交易

调用：`wormholed-cli whc_listpendingtransactions address`

参数：过滤地址

返回值：指定地址未确认的交易信息列表

示例如下
```
wormholed-cli whc_listpendingtransactions qpadl79yr4hh0fym4gw73mxp3rm4325knqe7fj6a9s
[
]
```

### whc_listproperties
描述：列出整个系统的所有token信息

调用：`wormholed-cli whc_listproperties`

返回值：系统的所有token列表

示例如下
```
wormholed-cli whc_listproperties
[
  {
    "propertyid": 1,
    "name": "WHC",
    "category": "N/A",
    "subcategory": "N/A",
    "data": "WHC serve as the binding between Bitcoin cash, smart properties and contracts created on the Wormhole.",
    "url": "http://www.wormhole.cash",
    "precision": 0
  },
  {
    "propertyid": 3,
    "name": "BFT",
    "category": "BitApp",
    "subcategory": "Blockchain",
    "data": "BitApp Founder Token",
    "url": "ht\ntps://www.bitapp.pro",
    "precision": 0
  },
  {
    "propertyid": 4,
    "name": "WHD",
    "category": "group",
    "subcategory": "coprenet",
    "data": "the mainnet token issued",
    "url": "www.wormhole.cash",
    "precision": 8
  }
]
```

### whc_listtransactions
描述：列出块链上与当前节点钱包地址相关的所有交易信息

调用：`wormholed-cli  whc_listtransactions (address, count, skip, startblock, endblock)`

参数：该RPC的所有参数都是可选填的

*   address ：过滤的地址
*   count ：获取的交易个数
*   skip ：跳过前n个交易
*   startblock ：交易开始的块号
*   endblock ：交易结束的块号

返回值：交易信息列表
示例如下
```
wormholed-cli  whc_listtransactions qz04wg2jj75x34tge2v8w0l6r0repfcvcygv3t7sg5
[
]
```

### whc_getfrozenbalance

解释：按地址与资产ID查询wormhole冻结资产余额

调用：`wormholed-cli whc_getfrozenbalance "frozenaddress" propertyid    `

参数：

- frozenaddress：冻结资产地址，CashAddr格式
- propertyid：资产ID

返回值：冻结资产余额信息

示例如下

```java
wormholed-cli whc_getfrozenbalance qqpj0yu8w9ukg7x4h83xx7a4nj8f7mssh5dgn6flfu 320
{
  "frozen": true,
  "balance": "1900.00000000"
}
```

返回值字段描述

- frozen ：指定资产是否被冻结
- balance ：被冻结资产余额

### whc_getfrozenbalanceforid

解释：按资产ID查询wormhole冻结资产余额

调用：`wormholed-cli whc_getfrozenbalanceforid propertyid    `

参数：

- propertyid：资产ID

返回值：冻结资产余额信息

示例如下

```java
wormholed-cli whc_getfrozenbalanceforid 320
[
  {
    "address": "bchtest:qqpj0yu8w9ukg7x4h83xx7a4nj8f7mssh5dgn6flfu",
    "balance": "1900.00000000"
  }
]
```

返回值字段描述

- address ：被冻结地址
- balance ：被冻结资产余额

### whc_getfrozenbalanceforaddress

解释：按地址查询wormhole冻结资产余额

调用：`wormholed-cli whc_getfrozenbalanceforaddress "address"    `

参数：

- address：待查询地址

返回值：冻结资产余额信息

示例如下



## 交易创建

下述提供两种方案来创建交易

方案一：调用表二中的RPC，可以直接创建wormhole交易；这系列RPC调用对节点有如下要求
*   要求wormhole节点必须有一个可以使用的钱包
*   钱包中必须有足够的BCH和WHC，可以用来从创建交易
*   优点是：调用接口简单，可以供节点用户直接使用
*   缺点是：必须在当前节点有一个 含有BCH和WHC的钱包

方案二：通过表三和表四RPC的组合调用，也可以用来创建wormhole交易
*   本调用方案可以用来开发钱包等应用，通过调用服务器端的RPC服务，生成未签名的交易；然后钱包进行签名，向外发送签名后的交易
*   优点是：不要求调用节点必须含有钱包，不用担心token遗失问题；允许线下签名，广播交易
*   缺点是：调用流程稍加繁琐

## 表二 ：创建wormhole交易

RPC |feature |
---|----
whc_burnbchgetwhc | 燃烧BCH，获取WHC
whc_sendissuancefixed | 发行固定属性的token
whc_sendissuancemanaged | 发行可管理的token
whc_sendissuancecrowdsale | 发行可众筹的token
whc_particrowsale | 参与众筹
whc_sendclosecrowdsale | 关闭众筹
whc_sendgrant | 增发管理token的token数量
whc_sendrevoke | 销毁管理token的token数量
whc_send | 转账
whc_sendsto | 空投
whc_sendall | 发送指定地址的所有token至另一个地址
whc_sendchangeissuer | 修改token的发行者
whc_sendfreeze | 冻结可管理token
whc_sendunfreeze | 解冻可管理token

### whc_burnbchgetwhc
描述：燃烧BCH，获取WHC

调用：`wormholed-cli whc_burnbchgetwhc "amount" (redeemaddress)`

参数

*   amount ： 燃烧的BCH金额
*   redeemaddress ：多余的BCH赎回地址；可选项，默认有钱包生成一个新地址。

返回值：生成的交易哈希

示例如下
```
wormholed-cli whc_burnbchgetwhc 1.5

153438e063d2533f6337d86b9ab7494cf907c4e927c3cef7a9358504cb049cf6
```


### whc_sendissuancefixed
描述：发行固定属性的token

调用：`wormholed-cli whc_sendissuancefixed "fromaddress" ecosystem precision previousid "category" "subcategory" "name" "url" "data" "totalNumber"`

参数

*   fromaddress ：发行固定属性token的地址
*   ecosystem ：token的生态体系；当前必须为1
*   precision ：token的精度
*   previousid ：附加价值的tokenID；当前必须为0
*   category ：token的类别
*   subcategory ：token的子类别
*   name ：token的名称
*   url ：token的URL
*   data ：token的自定义描述数据
*   totalNumber ：发行token的数量

返回值：生成的交易哈希
示例如下
```
wormholed-cli whc_sendissuancefixed  qz08vwmzp6zy6h5jvgrt556d9f9e08a32y5eqaqztq 1 3 0 "company" "compute" "luzhiyao" "www.ludete.com" "hello world" 10082936279.232

1d059313c873018a2f0dfe855ba1dde0ce19ec50db51eca42236baa1b8c8d6f4
```

### whc_sendissuancemanaged
描述：发行可管理的token

调用：`wormholed-cli whc_sendissuancemanaged "fromaddress" ecosystem precision previousid "category" "subcategory" "name" "url" "data"`

参数

*   fromaddress ：发行可管理token的地址
*   ecosystem ：token的生态体系；当前必须为1
*   precision ：token的精度
*   previousid ：附加价值的tokenID；当前必须为0
*   category ：token的类别
*   subcategory ：token的子类别
*   name ：token的名称
*   url ：token的URL
*   data ：token的自定义描述数据

返回值：生成的交易哈希

示例如下
```
wormholed-cli whc_sendissuancemanaged  qz08vwmzp6zy6h5jvgrt556d9f9e08a32y5eqaqztq 1 3 0 "company" "compute" "luzhiyao" "www.ludete.com" "hello world"

ae878af640344f3c8fae85ed4d37eb0f2a77a2553a0cb7645ff7c92d23d89768
```

### whc_sendissuancecrowdsale
描述：发行可众筹的token

调用：`wormholed-cli whc_sendissuancecrowdsale "fromaddress" ecosystem precision previousid "category" "subcategory" "name" "url" "data" propertyiddesired tokensperunit deadline  earlybonus undefine totalNumber`

参数

*   fromaddress ：发行可管理token的地址
*   ecosystem ：token的生态体系；当前必须为1
*   precision ：token的精度
*   previousid ：附加价值的tokenID；当前必须为0
*   category ：token的类别
*   subcategory ：token的子类别
*   name ：token的名称
*   url ：token的URL
*   data ：token的自定义描述数据
*   propertyiddesired ：募集的tokenID
*   tokensperunit ：众筹token的单价；1WHC等于多少token，单价的范围[10<sup>-8</sup>, 10<sup>8</sup>]
*   deadline : 众筹的截止时间
*   earlybonus ：早鸟激励
*   undefine ：未定义字段，必须为0
*   totalNumber ：发行众筹的数量

返回值：生成的交易哈希

示例如下
```
wormholed-cli whc_sendissuancecrowdsale  qz08vwmzp6zy6h5jvgrt556d9f9e08a32y5eqaqztq 1 3 0 "company" "compute" "luzhiyao" "www.ludete.com" "hello world" 1 1002 1536565622 1 0 21231242131

e7d2c232a91a3c1855cbfed05bf75e31041676898b37fa93420308cb3ff7a666
```

### whc_particrowsale 
描述：参与众筹

调用：`wormholed-cli whc_particrowsale "fromaddress" "toaddress"  "amount" ( "redeemaddress" "referenceamount" )`

参数

*   fromaddress ：参与众筹的地址
*   toaddress ： 众筹发行者的地址
*   amount ：参与众筹的WHC金额
*   redeemaddress ：赎回多余BCH的地址；可选，默认为参与众筹的地址
*   referenceamount ：给众筹发行者的BCH的金额；可选，默认为系统的最小金额

返回值：生成的交易哈希

示例如下
```
wormholed-cli whc_particrowsale qpalmy832fp9ytdlx444sehajljnm554dulckcvjl5  qz08vwmzp6zy6h5jvgrt556d9f9e08a32y5eqaqztq 100

d0a21fd05ed0e2f23f594d77b8ecec96a94ad1dec2785b658e4f705504766cda
```

### whc_sendclosecrowdsale
描述：关闭众筹

调用：`wormholed-cli whc_sendclosecrowdsale "fromaddress" propertyid`

参数：

*   fromaddress ：众筹发行者的地址
*   propertyid ：众筹的tokenID

返回值：生成的交易哈希

示例如下
```
wormholed-cli whc_sendclosecrowdsale qpalmy832fp9ytdlx444sehajljnm554dulckcvjl5 11

967328ba4b60876e0b1039e7e4ac77c2d7678ac98e968599786a68df18f353cf
```

### whc_sendgrant
描述：增发管理token的数量

调用：`wormholed-cli whc_sendgrant "fromaddress" "toaddress" propertyid "amount" ( "memo" )`

参数：

*   fromaddress ：token的发行者地址
*   toaddress ：向该地址增发指定数量的token
*   propertyid ：可管理tokenID
*   amount ：增发的token数量
*   memo ：增发token的自定义信息

返回值：生成的交易哈希

示例如下
```
wormholed-cli whc_sendgrant qz08vwmzp6zy6h5jvgrt556d9f9e08a32y5eqaqztq qpalmy832fp9ytdlx444sehajljnm554dulckcvjl5 115 1242

ed76f90ef3950cac5198045a009483dc90d3ce8a4c8d491d86127b1b3f55a555
```


### whc_sendrevoke
描述：销毁管理token的数量

调用：`wormholed-cli whc_sendrevoke "fromaddress" propertyid "amount" ( "memo" )`

参数：
    
*   fromaddress ：token的发行者地址
*   propertyid ：可管理tokenID
*   amount ：销毁的token数量
*   memo ：销毁token的自定义信息

返回值：生成的交易哈希

示例如下
```
wormholed-cli whc_sendrevoke qz08vwmzp6zy6h5jvgrt556d9f9e08a32y5eqaqztq 115 100

91c56172524fe11fabb7f954cf893a7c42be31e79f549031d11b89a5ea7d4581
```

### whc_send
描述：转账

调用：`wormholed-cli whc_send "fromaddress" "toaddress" propertyid "amount" ( "redeemaddress" "referenceamount" )`

参数：

*   fromaddress ：token的发送者地址
*   toaddress ：token的接收者地址
*   propertyid ：转账的tokenID
*   amount ：转账的token数量
*   redeemaddress ：赎回BCH地址；可选，默认为token发送者的地址
*   referenceamount ：token接收者输出的BCH金额；可选，默认为系统最小金额

返回值：生成的交易哈希

示例如下
```
wormholed-cli whc_send qz08vwmzp6zy6h5jvgrt556d9f9e08a32y5eqaqztq qpalmy832fp9ytdlx444sehajljnm554dulckcvjl5 115 100

d87ae34ed64e23087228eba458af1ebaf94f0db04912c59f6531f2b8c5c72f91
```

### whc_sendsto
描述：空投

调用：`wormholed-cli whc_sendsto  "fromaddress" propertyid "amount" ( "redeemaddress" distributionproperty )`

参数：

*   fromaddress ：空投的发送者地址
*   propertyid ：进行空投的tokenID
*   amount ：空投的token数量
*   redeemaddress ：赎回BCH地址；可选，默认为token发送者的地址
*   distributionproperty ：接收空投的目标tokenID；可选，默认为空投token的ID

返回值：生成的交易哈希

示例如下
```
wormholed-cli whc_sendsto  qz08vwmzp6zy6h5jvgrt556d9f9e08a32y5eqaqztq 115 1000 qz08vwmzp6zy6h5jvgrt556d9f9e08a32y5eqaqztq 1

bf3d30fc9c9424bdc6e38fc55320bad6cda9488e74296fc8dfb06cb2d9ee0fd9
```


### whc_sendall 
描述：发送指定地址的所有token至另一个地址

调用：`wormholed-cli whc_sendall "fromaddress" "toaddress" ecosystem ( "redeemaddress" "referenceamount" )`

参数：

*   fromaddress ：token发送者地址
*   toaddress ：token接收者地址
*   ecosystem ：生态体系；当前必须为1
*   redeemaddress ：赎回BCH地址；可选，默认为token发送者的地址
*   referenceamount ：token接收者输出的BCH金额；可选，默认为系统最小金额

返回值：生成的交易哈希

示例如下
```
wormholed-cli whc_sendall qz08vwmzp6zy6h5jvgrt556d9f9e08a32y5eqaqztq qpalmy832fp9ytdlx444sehajljnm554dulckcvjl5 1

cb93fbe852955201b757a790a73bb964728dd4309a449b2e46e67c9f69292909
```

### whc_sendchangeissuer 
描述：修改token的发行者

调用：`wormholed-cli whc_sendchangeissuer "fromaddress" "toaddress" propertyid`

参数：

*   fromaddress ：token的发行者
*   toaddress ：修改后的发行者
*   propertyid ：tokenID

返回值：生成的交易哈希

示例如下
```
wormholed-cli whc_sendchangeissuer qz08vwmzp6zy6h5jvgrt556d9f9e08a32y5eqaqztq qpalmy832fp9ytdlx444sehajljnm554dulckcvjl5 115

d1fb2ee670e3489e80f9fbfbd9e001dfb4ed64d5107354e7b74ceb0398625fb1
```

### whc_sendfreeze

解释：构建并发送wormhole冻结资产交易

调用：`wormholed-cli whc_sendfreeze "fromaddress" propertyid "amount" "frozenaddress"  `

调用者：该资产发行者

参数：

- fromaddress：资产发行者地址，CashAddr格式
- propertyid：资产ID
- amount：资产数量；当前可为任意值
- frozenaddress：冻结资产地址，CashAddr地址

返回值：交易哈希值

示例如下

```java
wormholed-cli whc_sendfreeze qpjua0mvqpnyxddavqys2j3d8wuewarmnvx3kqha2q 320 "100.0" qqpj0yu8w9ukg7x4h83xx7a4nj8f7mssh5dgn6flfu
45a9e5702c7d5c9836d77a5f571059a2b50fe32e39bf7bf0c0d7951392cb4e0b
```

### whc_sendunfreeze

解释：构建并发送wormhole解冻资产交易

调用：`wormholed-cli whc_sendunfreeze "fromaddress" propertyid "amount" "frozenaddress"  `

调用者：该资产发行者

参数：

- fromaddress：资产发行者地址，CashAddr格式
- propertyid：资产ID
- amount：资产数量；当前可为任意值
- frozenaddress：冻结资产地址，CashAddr地址

返回值：交易哈希值

示例如下

```java
wormholed-cli whc_sendunfreeze qpjua0mvqpnyxddavqys2j3d8wuewarmnvx3kqha2q 320 "100.0" qqpj0yu8w9ukg7x4h83xx7a4nj8f7mssh5dgn6flfu
4d7e239fbc1a71ce7b27ae7b6bc4c557131973505f0d1701377d0302177390f9
```

## 表三 ：创建wormhole协议的载荷数据

RPC |feature |
---|----
whc_createpayload_burnbch | 燃烧BCH，获取WHC
whc_createpayload_issuancefixed | 发行固定属性的token
whc_createpayload_issuancemanaged | 发行可管理的token
whc_createpayload_issuancecrowdsale | 发行可众筹的token
whc_createpayload_particrowdsale | 参与众筹
whc_createpayload_closecrowdsale | 关闭众筹
whc_createpayload_grant |  增发管理token的数量
whc_createpayload_revoke | 销毁管理token的数量
whc_createpayload_simplesend | 转账
whc_createpayload_sto | 空投
whc_createpayload_sendall | 发送指定地址的所有token至另一个地址
whc_createpayload_changeissuer | 修改token的发行者
whc_createpayload_freeze | 冻结资产交易载荷
whc_createpayload_unfreeze | 解冻资产交易载荷

### whc_createpayload_burnbch
描述：燃烧BCH，获取WHC

调用：`wormholed-cli whc_createpayload_burnbch`

返回值：生成的wormhole 协议载荷数据

示例如下
```
wormholed-cli whc_createpayload_burnbch

00000044
```

### whc_createpayload_issuancefixed
描述：发行固定属性的token

调用：`wormholed-cli whc_createpayload_issuancefixed ecosystem precision previousid "category" "subcategory" "name" "url" "data" "totalNumber"`

参数

*   ecosystem ：token的生态体系；当前必须为1
*   precision ：token的精度
*   previousid ：附加价值的tokenID；当前必须为0
*   category ：token的类别
*   subcategory ：token的子类别
*   name ：token的名称
*   url ：token的URL
*   data ：token的自定义描述数据
*   totalNumber ：发行token的数量

返回值：生成的wormhole 协议载荷数据

示例如下
```
wormholed-cli whc_createpayload_issuancefixed  1 3 0 "company" "compute" "luzhiyao" "www.ludete.com" "hello world" 10082936279.232

0000003201000300000000636f6d70616e7900636f6d70757465006c757a686979616f007777772e6c75646574652e636f6d0068656c6c6f20776f726c64000000092b9dd5d0c0
```

### whc_createpayload_issuancemanaged
描述：发行可管理的token

调用：`wormholed-cli whc_createpayload_issuancemanaged  ecosystem precision previousid "category" "subcategory" "name" "url" "data"`

参数

*   ecosystem ：token的生态体系；当前必须为1
*   precision ：token的精度
*   previousid ：资产冻结功能开关；默认为0，表示关闭冻结功能；置为1表示开启冻结功能
*   category ：token的类别
*   subcategory ：token的子类别
*   name ：token的名称
*   url ：token的URL
*   data ：token的自定义描述数据

返回值：生成的交易哈希

示例如下
```
wormholed-cli whc_createpayload_issuancemanaged   1 3 0 "company" "compute" "luzhiyao" "www.ludete.com" "hello world"

0000003601000300000000636f6d70616e7900636f6d70757465006c757a686979616f007777772e6c75646574652e636f6d0068656c6c6f20776f726c6400
```

注意：自wormhole-0.1.1版本起，previousid字段重新解释为资产冻结开关，合法值为0，1。默认为0（兼容之前版本，即关闭冻结功能），新增值1表示开启资产冻结功能。

### whc_createpayload_issuancecrowdsale

描述：发行可众筹的token

调用：`wormholed-cli whc_createpayload_issuancecrowdsale ecosystem precision previousid "category" "subcategory" "name" "url" "data" propertyiddesired tokensperunit deadline  earlybonus undefine totalNumber`

参数

*   ecosystem ：token的生态体系；当前必须为1
*   precision ：token的精度
*   previousid ：附加价值的tokenID；当前必须为0
*   category ：token的类别
*   subcategory ：token的子类别
*   name ：token的名称
*   url ：token的URL
*   data ：token的自定义描述数据
*   propertyiddesired ：募集的tokenID
*   tokensperunit ：众筹token的单价；1WHC等于多少token，单价的范围[10<sup>-8</sup>, 10<sup>8</sup>]
*   deadline : 众筹的截止时间
*   earlybonus ：早鸟激励
*   undefine ：未定义字段，必须为0
*   totalNumber ：发行众筹的数量

返回值：生成的交易哈希

示例如下
```
wormholed-cli whc_createpayload_issuancecrowdsale   1 3 0 "company" "compute" "luzhiyao" "www.ludete.com" "hello world" 1 1002 1536565622 1 0 21231242131

0000003301000300000000636f6d70616e7900636f6d70757465006c757a686979616f007777772e6c75646574652e636f6d0068656c6c6f20776f726c640000000001000000175462aa00000000005b96217601000000134f48a53638
```

### whc_createpayload_particrowdsale
描述：参与众筹

调用：`wormholed-cli whc_createpayload_particrowdsale  "amount" `

参数

*   amount ：参与众筹的WHC金额

返回值：生成的交易哈希

示例如下
```
wormholed-cli whc_createpayload_particrowdsale    100

000000010000000100000002540be400
```

### whc_createpayload_closecrowdsale
描述：关闭众筹

调用：`wormholed-cli whc_createpayload_closecrowdsale  propertyid`

参数：

*   propertyid ：众筹的tokenID

返回值：生成的交易哈希

示例如下
```
wormholed-cli whc_createpayload_closecrowdsale  11

000000350000000b
```


### whc_createpayload_grant
描述：增发管理token的数量

调用：`wormholed-cli whc_createpayload_grant  propertyid "amount" ( "memo" )`

参数：

*   propertyid ：可管理tokenID
*   amount ：增发的token数量
*   memo ：增发token的自定义信息

返回值：生成的交易哈希

示例如下
```
wormholed-cli whc_createpayload_grant  115 1242

0000003700000073000000000012f39000
```

### whc_createpayload_revoke
描述：销毁管理token的数量

调用：`wormholed-cli whc_createpayload_revoke  propertyid "amount" ( "memo" )`

参数：
    
*   propertyid ：可管理tokenID
*   amount ：销毁的token数量
*   memo ：销毁token的自定义信息

返回值：生成的交易哈希

示例如下
```
wormholed-cli whc_createpayload_revoke  115 100

000000380000007300000000000186a000
```

### whc_createpayload_simplesend
描述：转账

调用：`wormholed-cli whc_createpayload_simplesend  propertyid "amount" `

参数：

*   fromaddress ：token的发送者地址
*   toaddress ：token的接收者地址
*   propertyid ：转账的tokenID
*   amount ：转账的token数量
*   redeemaddress ：赎回BCH地址；可选，默认为token发送者的地址
*   referenceamount ：token接收者输出的BCH金额；可选，默认为系统最小金额

返回值：生成的交易哈希

示例如下
```
wormholed-cli whc_createpayload_simplesend   115 100

000000000000007300000000000186a0
```

### whc_createpayload_sto
描述：空投

调用：`wormholed-cli whc_createpayload_sto   propertyid "amount"  distributionpropertyß`

参数：

*   fromaddress ：空投的发送者地址
*   propertyid ：进行空投的tokenID
*   amount ：空投的token数量
*   redeemaddress ：赎回BCH地址；可选，默认为token发送者的地址
*   distributionproperty ：接收空投的目标tokenID；可选，默认为空投token的ID

返回值：生成的交易哈希

示例如下
```
wormholed-cli whc_createpayload_sto   115 1000  1

000000030000007300000000000f424000000001
```

### whc_createpayload_sendall
描述：发送指定地址的所有token至另一个地址

调用：`wormholed-cli whc_createpayload_sendall  ecosystem `

参数：

*   ecosystem ：生态体系；当前必须为1
*   redeemaddress ：赎回BCH地址；可选，默认为token发送者的地址
*   referenceamount ：token接收者输出的BCH金额；可选，默认为系统最小金额

返回值：生成的交易哈希

示例如下
```
wormholed-cli whc_createpayload_sendall   1

0000000401
```

### whc_createpayload_changeissuer
描述：修改token的发行者

调用：`wormholed-cli whc_createpayload_changeissuer  propertyid`

参数：

*   fromaddress ：token的发行者
*   toaddress ：修改后的发行者
*   propertyid ：tokenID

返回值：生成的交易哈希

示例如下
```
wormholed-cli  whc_createpayload_changeissuer   115

0000004600000073
```

### whc_createpayload_freeze

解释：构建wormhole冻结资产交易载荷

调用：`wormholed-cli whc_createpayload_freeze "toaddress" propertyid "amount"  `

参数：

- toaddress：待冻结地址，CashAddr格式
- propertyid：资产ID
- amount：资产数量；当前可为任意值

返回值：16进制交易载荷

示例如下

```java
wormholed-cli whc_createpayload_freeze qqpj0yu8w9ukg7x4h83xx7a4nj8f7mssh5dgn6flfu 320 "100"
000000b90000014000000002540be400626368746573743a7171706a307975387739756b6737783468383378783761346e6a3866376d7373683564676e36666c667500
```

### whc_createpayload_unfreeze

解释：构建wormhole解冻资产交易载荷

调用：`wormholed-cli whc_createpayload_unfreeze "toaddress" propertyid "amount"  `

参数：

- toaddress：待解冻地址，CashAddr格式
- propertyid：资产ID
- amount：资产数量；当前可为任意值

返回值：16进制交易载荷

示例如下

```java
wormholed-cli whc_createpayload_unfreeze qqpj0yu8w9ukg7x4h83xx7a4nj8f7mssh5dgn6flfu 320 "100"
000000ba0000014000000002540be400626368746573743a7171706a307975387739756b6737783468383378783761346e6a3866376d7373683564676e36666c667500
```

## 表四 ：创建交易

RPC |feature |
---|----
whc_createrawtx_input | 向未签名的交易追加一个交易输入
whc_createrawtx_opreturn | 将wormhole协议的载荷数据作为新输出的脚本追加在未签名交易中
whc_createrawtx_reference | 向未签名交易追加一个交易输出
whc_createrawtx_change | 向未签名交易输出集合的指定位置追加一个交易输出
whc_decodetransaction | 解析wormhole的原始交易

### whc_createrawtx_input
描述：向未签名的交易追加一个交易输入

调用：`wormholed-cli whc_createrawtx_input  "rawtx" "txid" c `

参数：

*   rawtx ：未签名的交易，可以为NULL
*   txid ：交易ID
*   txid ：要花费该交易(txid)的输出索引

返回值：添加交易输入后的交易数据

示例如下
```
wormholed-cli  whc_createrawtx_input   "" "d1fb2ee670e3489e80f9fbfbd9e001dfb4ed64d5107354e7b74ceb0398625fb1" 2

0200000001b15f629803eb4cb7e7547310d564edb4df01e0d9fbfbf9809e48e370e62efbd10200000000ffffffff0000000000
```

### whc_createrawtx_opreturn
描述：将wormhole协议的载荷数据作为新输出的脚本追加在未签名交易中

调用：`wormholed-cli wormhole "rawtx" "payload" `

参数：

*   rawtx ：未签名的交易，可以为NULL
*   payload ：wormhole协议的载荷数据

返回值：添加交易输出后的交易数据

示例如下
```
wormholed-cli whc_createrawtx_opreturn 0200000001b15f629803eb4cb7e7547310d564edb4df01e0d9fbfbf9809e48e370e62efbd10200000000ffffffff0000000000 0000004600000073

0200000001b15f629803eb4cb7e7547310d564edb4df01e0d9fbfbf9809e48e370e62efbd10200000000ffffffff0100000000000000000e6a0c08776863000000460000007300000000
```

### whc_createrawtx_reference
描述：向未签名交易追加一个交易输出

调用：`wormholed-cli whc_createrawtx_reference "rawtx"  "destination" ( amount )`

参数：

*   rawtx ：未签名的交易，可以为NULL
*   destination ：将要添加输出的的目的地址
*   amount ：该输出的金额；可选，默认为系统的最小金额

返回值：添加交易输出后的交易数据

示例如下
```
wormholed-cli whc_createrawtx_reference 0200000001b15f629803eb4cb7e7547310d564edb4df01e0d9fbfbf9809e48e370e62efbd10200000000ffffffff0100000000000000000e6a0c08776863000000460000007300000000 qz08vwmzp6zy6h5jvgrt556d9f9e08a32y5eqaqztq 1.24

0200000001b15f629803eb4cb7e7547310d564edb4df01e0d9fbfbf9809e48e370e62efbd10200000000ffffffff0200000000000000000e6a0c08776863000000460000007300176407000000001976a9149e763b620e844d5e926206ba534d2a4b979fb15188ac00000000
```

### whc_createrawtx_change
描述：向未签名交易输出集合的指定位置追加一个交易输出

调用：`wormholed-cli whc_createrawtx_change  "rawtx" "prevtxs" "destination" fee ( position ) `

参数：

*   rawtx ：未签名的交易，可以为NULL
*   prevtxs ： 未签名交易的引用输入集合
*   destination ：将要添加输出的的目的地址
*   fee ：交易费
*   position ：将这个交易输出追加在输出集合的指定位置；可选，默认追加在索引为0的位置。

返回值：添加交易输出后的交易数据

示例如下
```
wormholed-cli whc_createrawtx_change "0100000001b15ee60431ef57ec682790dec5a3c0d83a0c360633ea8308fbf6d5fc10a779670400000000ffffffff025c0d00000000000047512102f3e471222bb57a7d416c82bf81c627bfcd2bdc47f36e763ae69935bba4601ece21021580b888ff56feb27f17f08802ebed26258c23697d6a462d43fc13b565fda2dd52aeaa0a0000000000001976a914946cb2e08075bcbaf157e47bcb67eb2b2339d24288ac00000000" "[{\"txid\":\"6779a710fcd5f6fb0883ea3306360c3ad8c0a3c5de902768ec57ef3104e65eb1\",\"vout\":4,\"scriptPubKey\":\"76a9147b25205fd98d462880a3e5b0541235831ae959e588ac\",\"value\":0.00068257}]" "qz08vwmzp6zy6h5jvgrt556d9f9e08a32y5eqaqztq" 0.00003500 1

0100000001b15ee60431ef57ec682790dec5a3c0d83a0c360633ea8308fbf6d5fc10a779670400000000ffffffff035c0d00000000000047512102f3e471222bb57a7d416c82bf81c627bfcd2bdc47f36e763ae69935bba4601ece21021580b888ff56feb27f17f08802ebed26258c23697d6a462d43fc13b565fda2dd52aeefe40000000000001976a9149e763b620e844d5e926206ba534d2a4b979fb15188acaa0a0000000000001976a914946cb2e08075bcbaf157e47bcb67eb2b2339d24288ac00000000
```



## 表三，表四 RPC组合调用流程

### 燃烧BCH，获取基础货币RPC调用流程
1. 添加交易输入：   `wormholed-cli whc_createrawtx_input `
2. 创建输出，向指定的燃烧地址打币： `wormholed-cli whc_createrawtx_reference`
3. 创建燃烧BCH的wormhole载荷数据：  `wormholed-cli whc_createpayload_burnbch`
4. 创建输出，将创建的wormhole载荷数据添加进交易输出： `wormholed-cli whc_createrawtx_opreturn`
5. 创建输出，进行找零： `wormholed-cli whc_createrawtx_reference`  (这步可以省略，多余的金额会全部作为交易费)
6. 对创建的交易进行签名：`wormholed-cli signrawtransaction`
7. 发送交易：`wormholed-cli sendrawtransaction`

示例如下
```
1. wormholed-cli whc_createrawtx_input "" cda3e90e9ad1cd73ef793263d4b38a2ff6b80c149c04b7faf5540aac35d837d4 2
返回值：0200000001d437d835ac0a54f5fab7049c140cb8f62f8ab3d4633279ef73cdd19a0ee9a3cd0200000000ffffffff0000000000
2. wormholed-cli whc_createrawtx_reference "0200000001d437d835ac0a54f5fab7049c140cb8f62f8ab3d4633279ef73cdd19a0ee9a3cd0200000000ffffffff0000000000" "bchtest:qqqqqqqqqqqqqqqqqqqqqqqqqqqqqdmwgvnjkt8whc" 2
返回值：0200000001d437d835ac0a54f5fab7049c140cb8f62f8ab3d4633279ef73cdd19a0ee9a3cd0200000000ffffffff0100c2eb0b000000001976a9140000000000000000000000000000000000376e4388ac00000000
3. wormholed-cli whc_createpayload_burnbch
返回值：00000044
4. wormholed-cli whc_createrawtx_opreturn "0200000001d437d835ac0a54f5fab7049c140cb8f62f8ab3d4633279ef73cdd19a0ee9a3cd0200000000ffffffff0100c2eb0b000000001976a9140000000000000000000000000000000000376e4388ac00000000" "00000044"
返回值：0200000001d437d835ac0a54f5fab7049c140cb8f62f8ab3d4633279ef73cdd19a0ee9a3cd0200000000ffffffff0200c2eb0b000000001976a9140000000000000000000000000000000000376e4388ac00000000000000000a6a08087768630000004400000000
5. wormholed-cli whc_createrawtx_reference "0200000001d437d835ac0a54f5fab7049c140cb8f62f8ab3d4633279ef73cdd19a0ee9a3cd0200000000ffffffff0200c2eb0b000000001976a9140000000000000000000000000000000000376e4388ac00000000000000000a6a08087768630000004400000000" "qpnprg0h9y8ts3p9257f3sfe7j040yemqql84kh26q" 2.999
返回值：0200000001d437d835ac0a54f5fab7049c140cb8f62f8ab3d4633279ef73cdd19a0ee9a3cd0200000000ffffffff0300c2eb0b000000001976a9140000000000000000000000000000000000376e4388ac00000000000000000a6a080877686300000044601ce011000000001976a9146611a1f7290eb84425553c98c139f49f57933b0088ac00000000
6. wormholed-cli signrawtransaction 0200000001d437d835ac0a54f5fab7049c140cb8f62f8ab3d4633279ef73cdd19a0ee9a3cd0200000000ffffffff0300c2eb0b000000001976a9140000000000000000000000000000000000376e4388ac00000000000000000a6a080877686300000044601ce011000000001976a9146611a1f7290eb84425553c98c139f49f57933b0088ac00000000
返回值：
{
  "hex": "0200000001d437d835ac0a54f5fab7049c140cb8f62f8ab3d4633279ef73cdd19a0ee9a3cd020000006b483045022100a70f30364283c1382d179f85a1f2b5a0bc26e8d11d6ccbd9dce4bde02bb6fc3e02202d3876e4db506de74bc387e7fa6f57e6d8f84188f0e6e3ffa1ee2656d5213104412102cfdb34fee8eb0f17e5fe731094036327e645803050797620f46fc718dc5479d3ffffffff0300c2eb0b000000001976a9140000000000000000000000000000000000376e4388ac00000000000000000a6a080877686300000044601ce011000000001976a9146611a1f7290eb84425553c98c139f49f57933b0088ac00000000",
  "complete": true
}
7. wormholed-cli sendrawtransaction 0200000001d437d835ac0a54f5fab7049c140cb8f62f8ab3d4633279ef73cdd19a0ee9a3cd020000006b483045022100a70f30364283c1382d179f85a1f2b5a0bc26e8d11d6ccbd9dce4bde02bb6fc3e02202d3876e4db506de74bc387e7fa6f57e6d8f84188f0e6e3ffa1ee2656d5213104412102cfdb34fee8eb0f17e5fe731094036327e645803050797620f46fc718dc5479d3ffffffff0300c2eb0b000000001976a9140000000000000000000000000000000000376e4388ac00000000000000000a6a080877686300000044601ce011000000001976a9146611a1f7290eb84425553c98c139f49f57933b0088ac00000000
返回值 ：2b932bf1e73a31e87ce30be3a4f86b9d68beb9e61e9badf399474b95c32180eb

```

### 转账
1. 添加交易输入：   `wormholed-cli whc_createrawtx_input `
2. 创建发送Token的载荷数据：    `wormholed-cli whc_createpayload_simplesend`
3. 创建交易输出，将创建的wormhole载荷数据添加进交易输出：   `wormholed-cli whc_createrawtx_opreturn`
4. 创建输出，进行找零：`wormholed-cli whc_createrawtx_reference`
5. 创建输出，将token转入该地址：`wormholed-cli whc_createrawtx_reference`
6. 对创建的交易进行签名：`wormholed-cli signrawtransaction`
7. 发送交易：`wormholed-cli sendrawtransaction`

### 转移指定账户的所有的token
1. 添加交易输入：   `wormholed-cli whc_createrawtx_input `
2. 创建发送token的载荷数据：    `wormholed-cli whc_createpayload_sendall`
3. 创建交易输出，将创建的wormhole载荷数据添加进交易输出：   `wormholed-cli whc_createrawtx_opreturn`
4. 创建输出，进行找零：`wormholed-cli whc_createrawtx_reference`（这步可以省略）
5. 创建输出，将token转入该地址：`wormholed-cli whc_createrawtx_reference`
6. 对创建的交易进行签名：`wormholed-cli signrawtransaction`
7. 发送交易：`wormholed-cli sendrawtransaction`

### 创建固定属性的token
1. 添加交易输入：   `wormholed-cli whc_createrawtx_input `
2. 生成创建固定token的载荷数据： `wormholed-cli whc_createpayload_issuancefixed`
3. 创建交易输出，将创建的wormhole载荷数据添加进交易输出：   `wormholed-cli whc_createrawtx_opreturn`
4. 创建输出，进行找零：`wormholed-cli whc_createrawtx_reference`
结果：此时创建的新token会在 第一个交易输入的地址上(即：在第一步引入)。
6. 对创建的交易进行签名：`wormholed-cli signrawtransaction`
7. 发送交易：`wormholed-cli sendrawtransaction`

### 创建众筹token
1. 添加交易输入：   `wormholed-cli whc_createrawtx_input `
2. 生成创建众筹token的载荷数据： `wormholed-cli whc_createpayload_issuancecrowdsale`
3. 创建交易输出，将创建的wormhole载荷数据添加进交易输出：   `wormholed-cli whc_createrawtx_opreturn`
4. 创建输出，进行找零：`wormholed-cli whc_createrawtx_reference`
结果：此时创建的新token会在 第一个交易输入的地址上(即：在第一步引入)。
6. 对创建的交易进行签名：`wormholed-cli signrawtransaction`
7. 发送交易：`wormholed-cli sendrawtransaction`

### 参与众筹
1. 添加交易输入：   `wormholed-cli whc_createrawtx_input `
2. 生成参与众筹的载荷数据：`wormholed-cli whc_createpayload_particrowdsale`
3. 创建交易输出，将创建的wormhole载荷数据添加进交易输出：   `wormholed-cli whc_createrawtx_opreturn`
4. 创建输出，进行找零：`wormholed-cli whc_createrawtx_reference`
5. 创建一个众筹的地址输出：`wormholed-cli whc_createrawtx_reference`
6. 对创建的交易进行签名：`wormholed-cli signrawtransaction`
7. 发送交易：`wormholed-cli sendrawtransaction`


### 关闭众筹
1. 添加交易输入：   `wormholed-cli whc_createrawtx_input `
2. 生成关闭众筹的载荷数据： `wormholed-cli whc_createpayload_closecrowdsale`
3. 创建交易输出，将创建的wormhole载荷数据添加进交易输出：   `wormholed-cli whc_createrawtx_opreturn`
4. 创建输出，进行找零：`wormholed-cli whc_createrawtx_reference`
6. 对创建的交易进行签名：`wormholed-cli signrawtransaction`
7. 发送交易：`wormholed-cli sendrawtransaction`

### 创建可管理token
1. 添加交易输入：   `wormholed-cli whc_createrawtx_input `
2. 生成创建可管理token的载荷数据： `wormholed-cli whc_createpayload_issuancemanaged`
3. 创建交易输出，将生成的wormhole载荷数据添加进交易输出：   `wormholed-cli whc_createrawtx_opreturn`
4. 创建输出，进行找零：`wormholed-cli whc_createrawtx_reference` 
6. 对创建的交易进行签名：`wormholed-cli signrawtransaction`
7. 发送交易：`wormholed-cli sendrawtransaction`

### 增发token
1. 添加交易输入：   `wormholed-cli whc_createrawtx_input `
2. 生成增发token的载荷数据： `wormholed-cli whc_createpayload_grant`
3. 创建交易输出，将创建的wormhole载荷数据添加进交易输出：   `wormholed-cli whc_createrawtx_opreturn`
4. 创建输出，进行找零：`wormholed-cli whc_createrawtx_reference`
5. 创建增发地址的输出：`wormholed-cli whc_createrawtx_reference`；如果向token的发行者地址增发，这步可以省略
6. 对创建的交易进行签名：`wormholed-cli signrawtransaction`
7. 发送交易：`wormholed-cli sendrawtransaction`

### 销毁token
1. 添加交易输入：   `wormholed-cli whc_createrawtx_input `
2. 生成销毁token的载荷数据： `wormholed-cli whc_createpayload_revoke `
3. 创建交易输出，将生成的wormhole载荷数据添加进交易输出：   `wormholed-cli whc_createrawtx_opreturn`
4. 创建输出，进行找零：`wormholed-cli whc_createrawtx_reference`
6. 对创建的交易进行签名：`wormholed-cli signrawtransaction`
7. 发送交易：`wormholed-cli sendrawtransaction`

### 进行空投
1. 添加交易输入：   `wormholed-cli whc_createrawtx_input `(注意：第一个输入必须含有足够的空投token)
2. 生成空投的载荷数据： `wormholed-cli whc_createpayload_sto`
3. 创建交易输出，将生成的wormhole载荷数据添加进交易输出：   `wormholed-cli whc_createrawtx_opreturn`
4. 创建输出，进行找零：`wormholed-cli whc_createrawtx_reference`
6. 对创建的交易进行签名：`wormholed-cli signrawtransaction`
7. 发送交易：`wormholed-cli sendrawtransaction`

### 更改token的发行者
1. 添加交易输入：   `wormholed-cli whc_createrawtx_input `(注意：第一个输入必须为token的发行地址)
2. 生成修改发行者的载荷数据： `wormholed-cli whc_createpayload_changeissuer`
3. 创建交易输出，将生成的wormhole载荷数据添加进交易输出：   `wormholed-cli whc_createrawtx_opreturn`
4. 创建输出，进行找零：`wormholed-cli whc_createrawtx_reference`
5. 创建token接收者输出：`wormholed-cli whc_createrawtx_reference`
6. 对创建的交易进行签名：`wormholed-cli signrawtransaction`
7. 发送交易：`wormholed-cli sendrawtransaction`

### 冻结token

1. 添加交易输入：   `wormholed-cli whc_createrawtx_input `
2. 生成冻结可管理token的载荷数据： `wormholed-cli whc_createpayload_freeze`
3. 创建交易输出，将生成的wormhole载荷数据添加进交易输出：   `wormholed-cli whc_createrawtx_opreturn`
4. 创建输出，进行找零：`wormholed-cli whc_createrawtx_reference` 
5. 对创建的交易进行签名：`wormholed-cli signrawtransaction`
6. 发送交易：`wormholed-cli sendrawtransaction`

### 解冻token

1. 添加交易输入：   `wormholed-cli whc_createrawtx_input `
2. 生成解冻可管理token的载荷数据： `wormholed-cli whc_createpayload_unfreeze`
3. 创建交易输出，将生成的wormhole载荷数据添加进交易输出：   `wormholed-cli whc_createrawtx_opreturn`
4. 创建输出，进行找零：`wormholed-cli whc_createrawtx_reference` 
5. 对创建的交易进行签名：`wormholed-cli signrawtransaction`
6. 发送交易：`wormholed-cli sendrawtransaction`