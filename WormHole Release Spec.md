

## 注意
众筹算法正在调整中，**当前众筹不可以使用**

由于协议字段的变动，块高度: 1249116之前，测试网创建的所有资产都废弃;

## 综述
该版本是对第一版协议的改进，主要为下述协议的变动
>*   Field: Property type
    *   去掉该字段，增加 Pricision 字段   
*   Field: Number of coins
    *   受上述字段的变动，三种类型的资产创建`Property type` 都修改为`Property Pricision`
    *   主要为总金额(Number of coins)受(Property type)的影响，允许资产的总金额有小数位。
*   Field: Transaction type
    *   增加了参与众筹功能
    *   修改了转账类型的功能
    *   创建众筹资产的功能
*   RPC调用接口有修改
    

## 协议字段定义

### Field: Property Pricision
描述：**Wormhole**协议中资产的精度标识
字节：uint16_t; 2 bytes
有效值：范围[0, 8]；分别表示小数点精度
>   0: 表示无小数点
1: 表示小数点后精度为1
.....
8: 表示小数点后精度为8


### Field: Number of coins
描述: 协议中用来存储Token的数量，示例如下:
>   * 对于小数点后有精度的Token，该字段的值除以精度(`Transaction type `)标识Token的数量；(如精度为1的Token，该字段存储的数值为10，标识Token的金额数量为1)
>   * 对于不可分割的Token(即精度`Transaction type`为0)，该字段的值就表示Token的金额；(如该字段存储的值为100，标识Token的金额数量为100)
 
字节：int64_t; 8 bytes
有效值：1-9,223,372,036,854,775,807
>   * 对于精度为8的Token金额范围: 0.00000001 -  92,233,720,368.54775807
>   * 对于精度为7的Token金额范围: 0.0000001 -  922,337,203,685.4775807
......
>   * 对于不可分割(精度为0)的Token金额范围：1 - 9,223,372,036,854,775,807

### Field: Transaction type
描述: **Wormhole**协议当前提供的功能
字节：uint16_t; 2 bytes 有效值;
有效值：新增了一种交易类型；重新修改了转账类型
>* 0: Token转账
* 1: 参与众筹 


## Wormhole协议的具体规范

下述的所有可以使用的方案
方案一：都要求服务器端有一个可以使用的钱包。
方案二：可以用来开发钱包等应用，通过调用服务器端的RPC服务，生成未签名的交易；然后钱包进行签名，向外发送签名后的交易。

**Wormhole**协议对交易输入、输出顺序的要求
在**Wormhole**系统中，对顺序的要求主要分为两类：

category | txin | txout 
----|----|----
WHC的发行交易(68)|第一个交易输入(索引为0)为发送者|第一个交易输出(索引为0)为燃烧输出，第二个输出(索引为1)为**Wormhole**协议|
其他类型的交易|第一个交易输入(索引为0)为发送者|最后一个交易输出(索引最大)为接收者|


### Token转账(0)
从指定的账户地址向另一个账户地址**转移**指定**金额**的**某种Token**。

注意: **最后一个输出必须为Token的接收者，第一个输入必须为Token的发送者**

修改的地方: 
**omnicore**协议中该类型的交易主要有两种功能

*   转账
*   参与众筹：通过向一个正在的众筹地址转账来实现参与众筹

**Wormhole**协议对该类型进行修改，只存在一种功能

*   转账

对于WHC资产的转账: `Number of Coins`, 该字段标识的单位为WHC, (如: 转1WHC，该字段值为1)


转账交易的规范：
```
交易输入: **Wormhole**引擎默认第一个交易输入(索引为0)为发送Token的账户地址(即发送者);
交易输出: **Wormhole**引擎默认最后一个交易输出(索引最大)为接收Token的账户地址(即接收者);
转账金额: 发送者向接收者转移的Token金额，发送者账户必须具有足够的只能;
转账的TokenID: 该Token必须在系统存在。
交易规范中主要涉及: **交易输入、输出的顺序，转账金额，TokenID**
```
**Wormhole** 该类型交易协议字段如下
Filed | Type | Example|
----| ---| ---|
版本(Transaction version) | Transaction version | 0
交易类型(Transaction type) | Transaction type | 68
TokenID(Property ID) | Currency identifier| 1(WHC)
转账金额(Amount to transfer)|Number of Coins|1(标识转1WHC)

除**Wormhole**协议的字段限制之外，存在以下情况的也为无效的**Wormhole**交易
>   * 发送者的地址上，指定Token的可用余额为0
>   * 转账的金额超过了发送者可以使用的Token金额
>   * 转账Token不存在
>   * 转账的TokenID为0(BCH)；该交易类型不支持BCH的转账。

操作示例如下：如想转账1WHC两种方式。

* 方案一：
`Wormholed-cli whc_send fromaddress toaddress propertyid amount (redeemaddress) (referenceamount)`  
referenceamount: toaddress输出上的金额，默认为最小的输出金额
redeemaddress: 多余的BCH输入金额的赎回地址，默认是发送者的地址

* 方案二：
添加交易输入： `Wormholed-cli whc_createrawtx_input "" txid index`
创建发送Token的载荷数据： `Wormholed-cli whc_createpayload_simplesend`
添加交易输出，将创建的**Wormhole**载荷数据添加进交易输出： ` Wormholed-cli whc_createrawtx_opreturn "rawtx" "payload"`
添加交易输出，进行找零：`Wormholed-cli whc_createrawtx_reference "rawtx" destination amount`
添加交易输出，将Token转入该地址：`Wormholed-cli whc_createrawtx_reference "rawtx" destination amount`
对创建的交易进行签名：`Wormholed-cli signrawtransaction`
发送交易：`Wormholed-cli sendrawtransaction`

**Wormhole**的有效载荷数据：6a140877686300000000000000010000000005f5e100
解释如下：
>   6a: OP_RETURN; 14: **Wormhole**协议的数据长度； 
>   08776863: **Wormhole**协议的magic
>   0000: 版本
>   0000: 交易类型
>   00000001: TokenID
>   0000000005f5e100: 转账金额（1WHC）

示例交易: d9dd78f140691dd946175696bd312dfcfac4c5cc5c4f5eb1143dfde0db83c25e

### 参与众筹(1)
参与指定的众筹交易；

注意: **最后一个输出必须为众筹发行者的地址，第一个输入必须为WHC的发送者**

参与众筹的交易的规范：
```
交易输入: **Wormhole**引擎默认第一个交易输入(索引为0)为发送Token的账户地址(即发送者);
交易输出: **Wormhole**引擎默认最后一个交易输出(索引最大)为接收Token的账户地址(即接收者);
转账金额: 发送者向接收者转移的Token金额，发送者账户必须具有足够的只能;
转账的TokenID: 该Token必须在系统存在。
交易规范中主要涉及: **交易输入、输出的顺序，转账金额，TokenID**
```

**Wormhole** 该类型交易协议字段如下
Filed | Type | Example|
----| ---| ---|
版本(Transaction version) |Transaction version | 0
交易类型(Transaction type) |Transaction type  | 1
TokenID(Property ID) | Currency identifier| 1(WHC)
参与众筹的金额(Amount to transfer)|Number of Coins|1(购买1WHC的token)


除**Wormhole**协议的字段限制之外，存在以下情况的也为无效的**Wormhole**交易
>   * 参与众筹的金额超过了发送者可以使用的Token金额
>   * 众筹地址不存在活跃的众筹


操作示例如下：如想使用1WHC参与众筹

* 方案一：
`Wormholed-cli whc_particrowsale fromaddress toaddress  amount (redeemaddress) (referenceamount)`  
amount: 参与众筹的WHC金额
referenceamount: toaddress输出上的金额，默认为最小的输出金额
redeemaddress: 多余的BCH输入金额的赎回地址，默认是发送者的地址

* 方案二：
添加交易输入： `Wormholed-cli whc_createrawtx_input "" txid index`
创建发送Token的载荷数据： `Wormholed-cli whc_createpayload_particrwosale`
添加交易输出，将创建的**Wormhole**载荷数据添加进交易输出： ` Wormholed-cli whc_createrawtx_opreturn "rawtx" "payload"`
添加交易输出，进行找零：`Wormholed-cli whc_createrawtx_reference "rawtx" destination amount`
添加交易输出，将Token转入该地址：`Wormholed-cli whc_createrawtx_reference "rawtx" destination amount`
对创建的交易进行签名：`Wormholed-cli signrawtransaction`
发送交易：`Wormholed-cli sendrawtransaction`

**Wormhole**的有效载荷数据：6a140877686300000001000000010000000005f5e100
解释如下：
>   6a: OP_RETURN; 14: **Wormhole**协议的数据长度； 
>   08776863: **Wormhole**协议的magic
>   0000: 版本
>   0001: 交易类型
>   00000001: TokenID
>   0000000005f5e100: 参与金额（1WHC）

### 创建众筹Token(51)
创建一笔众筹Token，一旦该笔交易被**Wormhole**节点确认有效，该发送者的地址上将会拥有一笔活跃的众筹。

注意：当前的众筹只允许 使用WHC参与。

修改的地方:

*   发行者奖励`Percentage for issuer` 变为 Undefined，该字段的值必须为0.
*   新增一个字段：众筹的总量


当下面的情况发生时，众筹将被永久关闭：
>   * 当块链的Tip块时间戳大于等于众筹截止时间`Deadline`时，该众筹将被永久关闭
>   * 众筹被手动强制关闭
>   * 众筹金额发售完毕

任何账户地址在指定时间只允许拥有一个活跃的众筹Token，避免众筹的参与者需要指定参加该账户的哪个众筹。

当其它账户发起交易参与该笔众筹Token时，一旦参与众筹的交易被**Wormhole**协议确认有效，购买的众筹会立即计入参与方账户的可使用资金中，参与方可以将购买来的众筹进行转账，出售，或其它用途；同时，这笔参与众筹的资金(WHC)，会立即计入众筹发行方账户的可使用资金中，众筹发行方可以这些募集来的资金。

为了激励众筹参与者尽早参加众筹，可以通过设立初始化的早鸟奖励机制，来奖励早期的参与者。这个早鸟奖励随着众筹启动的时间线性降低，直至众筹截止时，早鸟激励将为0.

早鸟奖励的百分比计算公式如下：**((众筹的截止时间-交易所在的区块时间)/每周的时间秒数 ) * 早鸟激励**
示例如下
>两种Token兑换比例：1WHC=100HLT; 早鸟奖励机制为10%, 如购买1WHC:
众筹参与者的交易在众筹截止两周前参与，购买者会获得20%的额外Token， 120HLT；在众筹截止时间1.5周前参与，购买者会获得15%的额外Token，115HLT。

**Wormhole**客户端保证计入众筹参与者的Token数量加上发行者账户的Token数量不超过总的发行量。**当一笔参与众筹的交易，资金不足购买众筹Token的最小精度时，该笔众筹交易失败，同时资金会返还给参与方。**

**Wormhole** 该类型交易协议字段如下
Filed | Type | Example|
----| ---| ---|
版本(Transaction version) |Transaction version|0
交易类型(Transaction type) |Transaction type|50
生态体系(Ecosystem)|Ecosystem|1
Token精度(Property Pricision)|Pricision|1
前TokenID(Previous Property ID) |Currency identifier|0
Token类别(Property Category)|String null-terminated|""
Token子类别(Property Subcategory)|String null-terminated|""
Token名称(Property Name)|String null-terminated|"ludete"
Token URL(Property URL)|String null-terminated|""
Token描述数据(Property Data)|String null-terminated|""
众筹募集的TokenID(Currency Identifier Desired) |Currency identifier|1
兑换比例(Number Properties per Unit Invested)|Number of Coins|100(募集Token:众筹Token)
众筹截止时间(Deadline)|Deadline|Unix时间戳
早鸟激励的百分比(Early Bird Bonus %/Week)|Integer one-byte | 10
Undefined |Integer one-byte | 必须为0
众筹的总量(totaltokens) | Number of coins| 10000.8

除**Wormhole**协议的字段限制之外，满足以下情况，该交易才被视为正确的**Wormhole**交易。
>   * 当`Property Type`字段标识新Token时，`Previous Property ID`字段必须为0
>   * `Property Name`字段不可以为NULL
>   * `Ecosystem`字段必须为1
>   * `Property Type`字段必须为1
>   * `Currency Identifier Desired`众筹的募集Token只允许是WHC(1)
>   * `Deadline`众筹的截止时间必须大于众筹的发起时间

操作示例如下：如想转账1WHC两种方式。

* 方案一：
`Wormholed-cli whc_sendissuancecrowdsale fromaddress ecosystem type previousid category subcategory name url data propertyiddesired Tokensperunit deadline earlybonus 0 totalTokens `

* 方案二：
添加交易输入：`Wormholed-cli whc_createrawtx_input "" txid index`
生成创建众筹Token的载荷数据：`Wormholed-cli whc_createpayload_issuancecrowdsale ecosystem type previousid category subcategory name url data propertyiddesired Tokensperunit deadline earlybonus 0 totalTokens`
添加交易输出，将创建的**Wormhole**载荷数据添加进交易输出：
`Wormholed-cli whc_createrawtx_opreturn "rawtx" "payload"`
添加交易输出，进行找零：`Wormholed-cli whc_createrawtx_reference "rawtx" "destination"  amount`
对创建的交易进行签名：`Wormholed-cli signrawtransaction "rawtx"`
发送交易：`Wormholed-cli sendrawtransaction "tx"`

**Wormhole**的有效载荷数据：6a 4c6c 08776863 0000 0033 01 0002 00000000 4372656174652043726f77642053616c6500426974436f696e00746573745f626974636f696e5f3000687474703a2f2f7777772e62616964752e636f6d0000
00000001 00000002540be400 000000005b881480 0a 00 000000000bebc200 

解释如下：
>   6a: OP_RETURN; 4c: OP_PUSHDATA1; 6c: **Wormhole**协议的数据长度； 
>   08776863: **Wormhole**协议的magic
>   0000: 版本
>   0033: 交易类型
>   01: 生态体系
>   0002: Token类别
>   00000000:前TokenID
>   .... : 中间的为一些自定义数据
>   00000001:众筹募集的TokenID
>   00000002540be400: 兑换比例
>   000000005b881480: 众筹截止时间
>   0a: 早鸟激励的百分比
>   00: Undefined filed
>   000000000bebc200 : 众筹的总金额
示例交易：e00b24013f4ef929dd554c3233c5f7df60e6249eab395b8c2540f01486470cb9


## RPC 调用接口的修改
主要针对WHC资产，进行了修改。
RPC name | callfunction | param|
----| ---- | ----
whc_createpayload_simplesend | whc_createpayload_simplesend | 第二个参数: 对于WHC资产，此处的传输单位为WHC |
whc_send | whc_send | 第四个参数: 对于WHC资产，此处的传输单位为WHC|
whc_getbalance | whc_getbalance| 返回值：对于WHC资产，所有返回的金额都已WHC为单位 |
whc_createpayload_particrwosale|whc_createpayload_particrwosale| 新增
whc_particrowsale| whc_particrowsale | 新增







