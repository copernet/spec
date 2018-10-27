
## Wormhole协议的解释
**Wormhole**协议是通过以**bitcoin cash**交易作为载体；使用**bitcoin cash**脚本中特殊的操作码`OP_RETURN`，来将**Wormhole**协议追加在该操作码后面。通俗来说：**Wormhole**交易是一种特殊的**bitcoin cash**交易，与**bitcoin cash**交易采用相同的安全、确认模型。

[**Wormhole**全节点客户端](https://github.com/copernet/wormhole)是[bitcoin-abc核心客户端](https://github.com/Bitcoin-ABC/bitcoin-abc)的超集，具有**bitcoin cash**协议+**Wormhole**协议两种功能。

**Wormhole**交易在核心客户端的处理流程:
接收到区块后---》**bitcoin cash** 模块确认该交易的合法性---》**Wormhole**模块来确认**Wormhole**交易的合法性。

**Wormhole**协议采用的是账户模型，每个**bitcoin cash** 地址是一个账户，每个账户地址可以含有多种token。


## 协议字段定义

### Field: Transaction version
描述：交易类型的处理版本；每种交易类型的版本单调递增，且相互独立

字节：uint16_t; 2 bytes

### Field: Transaction type
描述：**Wormhole**协议当前提供的功能

字节：uint16_t; 2 bytes

有效值：
*   0: token转账
*   1: 参与众筹
*   3: token空投
*   4: 转移账户的所有可用token
*   50: 创建固定数量的token
*   51: 创建可众筹的token
*   53: 强制关闭众筹
*   54: 创建可管理的token
*   55: 增发token
*   56: 销毁token
*   68: 获取系统的基础货币(WHC)
*   70: 修改token的发行人
*   185: 冻结token
*   186: 解冻token


### Field: Ecosystem
描述：该token所在的生态体系； 

字节：uint8_t; 1 byte 

有效值：当前有效值为1；

### Field: Number of coins
描述: 协议中用来存储Token的数量，示例如下:

*   对于小数点后有精度的Token，该字段的值除以精度(`proprety precision`)标识Token的数量；(如精度为1的Token，该字段存储的数值为10，标识Token的金额数量为1)
*   对于不可分割的Token(即精度`property pricison`为0)，该字段的值就表示Token的金额；(如该字段存储的值为100，标识Token的金额数量为100)

字节：int64_t; 8 bytes 

有效值：1-9,223,372,036,854,775,807

* 对于精度为8的Token金额范围: 0.00000001 - 92,233,720,368.54775807
* 对于精度为7的Token金额范围: 0.0000001 - 922,337,203,685.4775807 
* ......
* 对于不可分割(精度为0)的Token金额范围：1 - 9,223,372,036,854,775,807

### Field: Currency identifier
描述：系统中的tokenID 

字节：uint32_t; 4 bytes 

有效值：1, 3-2,147,483,647; 

0:BCH; 1:WHC; [3, 2,147,483,647]随后创建的TokenID，单调递增。


### Field: Property Precision
描述：Wormhole协议中资产的精度标识 

字节：uint16_t; 2 bytes 

有效值：范围[0, 8]；分别表示小数点精度
* 0: 表示无小数点 
* 1: 表示小数点后精度为1 
* ..... 
* 8: 表示小数点后精度为8

### Field: Integer-one byte

描述：用作乘数或作为其它计算的参数 

字节：uint8_t; 1 byte 

有效值：0-255

### Field: UTC Datetime

描述：Unix时间戳 

字节：uint64_t; 8 bytes

### Field: Freeze Feature Switch

描述：资产冻结功能开关 

字节：uint32_t; 4 bytes

有效值: 0,1

## Wormhole协议的具体规范

**Wormhole**协议对交易输入、输出顺序的要求

在**Wormhole**系统中，对顺序的要求主要分为两类：

category | txin | txout 
----|----|----
WHC的发行交易(68)|第一个交易输入(索引为0)为发送者|第一个交易输出(索引为0)为燃烧输出，第二个输出(索引为1)为**Wormhole**协议|
其他类型的交易|第一个交易输入(索引为0)为发送者|最后一个交易输出(索引最大)为接收者|


### WHC的发行(68)
通过燃烧BCH来获取WHC，BCH与WHC的兑换比例是1:100；通过向指定的燃烧地址发送BCH可获取WHC。
燃烧地址：

>   主网: bitcoincash:qqqqqqqqqqqqqqqqqqqqqqqqqqqqqu08dsyxz98whc

>   测试网: bchtest:qqqqqqqqqqqqqqqqqqqqqqqqqqqqqdmwgvnjkt8whc

在主网中，1000个区块确认后，WHC会到账；在测试网中，3个区块确认后，WHC会到账

>   回归测试网: bchreg:pqqqqqqqqqqqqqqqqqqqqqqqqqqqqp0kvc457r8whc

示例如下
>   假设在主链高度为 h1 = 55时，从address1地址燃烧了1BCH获取WHC; 在主链高度1054时，address1会收到100WHC; 在1054该区块高度后，即可以花费address1燃烧收到的100WHC。

**1BCH = 100WHC; 1WHC = 10000,0000C**

通过燃烧BCH获取WHC的交易规范:
对交易输入的规定：**Wormhole**引擎默认第一个交易输入(即索引为0)作为获取WHC的账户地址。

对交易输出的规定：**Wormhole**引擎默认：第一个交易输出必须为燃烧输出(即索引为0)；第二个输出为**Wormhole**协议的数据；第三个输出为多余的BCH赎回地址输出(这个输出可以忽略)；一个正确的燃烧交易，最少含有两个输出。

对燃烧金额的规定：最少燃烧1BCH。

交易规范中主要涉及：**交易输入、输出的顺序，燃烧金额，**Wormhole**载荷数据**

**Wormhole** 该类型交易协议字段如下:


Filed | Type | Example|
----| ---| ---|
版本(Transaction version) | Transaction version | 0
交易类型(Transaction type) | Transaction type | 68

除**Wormhole**协议的字段限制之外，存在以下情况的也为无效的**Wormhole**交易
* 燃烧BCH的输出不在索引为0的位置
* 索引为1的位置不是**Wormhole**的燃烧协议数据
* 燃烧的BCH金额小于1BCH

**Wormhole**的有效载荷数据：6a080877686300000044
解释如下：
```
6a: OP_RETURN; 08: Wormhole协议的载荷数据长度； 
08776863: Wormhole协议的magic
0000: 版本
0044: 交易类型
```

示例交易(测试链): 6b8b98f7248f96b168b8127e3eb31cabb00fd6a5eed5df6d012816d7b4335974

### Token转账(0)
从指定的账户地址向另一个账户地址**转移**指定**金额**的**某种Token**。

注意: **最后一个输出必须为Token的接收者，第一个输入必须为Token的发送者**

对于WHC资产的转账: `Number of Coins`, 该字段标识的单位为C, (如: 转1WHC，该字段值为100000000)

转账交易的规范：
```
交易输入: Wormhole引擎默认第一个交易输入(索引为0)为发送Token的账户地址(即发送者);
交易输出: Wormhole引擎默认最后一个交易输出(索引最大)为接收Token的账户地址(即接收者);
转账金额: 发送者向接收者转移的Token金额，发送者账户必须具有足够的资产;
转账的TokenID: 该Token必须在系统存在。
交易规范中主要涉及: 交易输入、输出的顺序，转账金额，TokenID
```

**Wormhole** 该类型交易协议字段如下:


Filed | Type | Example|
----| ---| ---|
版本(Transaction version) | Transaction version | 0
交易类型(Transaction type) | Transaction type | 68
TokenID(Property ID) | Currency identifier| 1(WHC)
转账金额(Amount to transfer)|Number of Coins|100000000(标识转1WHC)

除**Wormhole**协议的字段限制之外，存在以下情况的也为无效的**Wormhole**交易
* 转账的金额超过了发送者可以使用的Token金额
* 转账Token不存在
* 转账的TokenID为0(BCH)；该交易类型不支持BCH的转账

**Wormhole**的有效载荷数据：6a140877686300000000000000010000000005f5e100
解释如下：
```
6a: OP_RETURN; 14: Wormhole协议的载荷数据长度； 
08776863: Wormhole协议的magic
0000: 版本
0000: 交易类型
00000001: TokenID
0000000005f5e100: 转账金额（1WHC）
```

示例交易: 3e3368c0b345fb52507471ced2dcef1e2876597ac9981aa9520b75c87603cc5e

### 参与众筹(1)
参与指定的众筹交易；

注意: **最后一个输出必须为众筹发行者的地址，第一个输入必须为WHC的发送者**

参与众筹的交易的规范：
```
交易输入: Wormhole引擎默认第一个交易输入(索引为0)为发送Token的账户地址(即发送者);
交易输出: Wormhole引擎默认最后一个交易输出(索引最大)为众筹发行者的账户地址(即接收者);
购买金额: 发送者向接收者转移的Token金额，发送者账户必须具有足够的只能;
交易规范中主要涉及: **交易输入、输出的顺序，转账金额，TokenID**
```

**Wormhole** 该类型交易协议字段如下:


Filed | Type | Example|
----| ---| ---|
版本(Transaction version) |Transaction version | 0
交易类型(Transaction type) |Transaction type  | 1
TokenID(Property ID) | Currency identifier| 1(WHC)
参与众筹的金额(Amount to transfer)|Number of Coins|100000000(购买1WHC的token)


除**Wormhole**协议的字段限制之外，存在以下情况的也为无效的**Wormhole**交易
* 参与众筹的金额超过了发送者可以使用的Token金额
* 众筹地址不存在活跃的众筹
* 参与众筹的金额不足以购买众筹token的最小精度。

**Wormhole**的有效载荷数据：6a140877686300000001000000010000000005f5e100
解释如下：
```
6a: OP_RETURN; 14: Wormhole协议的载荷数据长度； 
08776863: Wormhole协议的magic
0000: 版本
0001: 交易类型
00000001: TokenID
0000000005f5e100: 参与金额（1WHC）
```
示例交易: ea9698f13179494b4c705d90517e800185093a24454e86addb10f59e95af06b4


### token空投(3)
将一种token(ID1)的指定金额**空投**给某种token(ID2)的持有人(除了发送者)，所有接受空投的地址获取的金额(ID1)按所含有的token(ID2)比例进行分配。
注意：**第一个输入必须为空投的发送者**

空投会收取一定的燃料费，以WHC计价，每场空投消耗的手续费为：接收空投的人数*系统单价(1C);

**Wormhole** 该类型交易协议字段如下:


Filed | Type | Example|
----| ---| ---|
版本(Transaction version) |Transaction version | 0
交易类型(Transaction type) |Transaction type  | 3
空投tokenID(Property ID) | Currency identifier | 1
空投金额(Amount to transfer)|Amount to transfer | 15000000000(150WHC)
目的tokenID(Property ID) |Currency identifier | 1

除**Wormhole**协议的字段限制之外，存在以下情况的也为无效的**Wormhole**交易
* 发送者的地址上可使用的金额小于空投的金额
* 指定的token不存在，或者tokenID为0
* 发送者的地址上没有足够的WHC支付燃料费
* 发送者地址上含有所有的目的token

**Wormhole**的有效载荷数据：6a18087768630000000300000001000000037e11d600
解释如下：
```
6a: OP_RETURN; 18: Wormhole协议的载荷数据长度； 
08776863: Wormhole协议的magic
0000: 版本
0003: 交易类型
00000001: 空投tokenID
000000037e11d600: 空投金额（1.5WHC）
00000001: 目的tokenID
```

示例交易：1db5d3b5c094e0d839465a1623d1b29baccb03e1bc2d817c6ee87d8b7ca4ebc4

### 转移账户的所有可用token(4)
转移一个账户地址(A)的所有token至另一个账户地址(B)，此处资产不包含BCH.
注意：**最后一个输出必须为token的接收者，第一个输入必须为token的发送者**

**Wormhole** 该类型交易协议字段如下:


Filed | Type | Example|
----| ---| ---|
版本(Transaction version) |Transaction version | 0
交易类型(Transaction type) |Transaction type  | 4
生态体系(Ecosystem)| Ecosystem |1

**Wormhole**的有效载荷数据：6a09087768630000000401
解释如下：
```
6a: OP_RETURN; 09: **Wormhole**协议的数据长度； 
08776863: Wormhole协议的magic
0000: 版本
0004: 交易类型
01: 生态体系
```

示例交易：6b6b04aed7ed091a9b1f89b9517064cb16cd5032f1b0994d93b6db2a5d20e871

### 创建固定数量的token(50)
创建指定数量的token，一旦该交易被**Wormhole**有效确认，创建者地址上会含有这些数量的token。

除**Wormhole**协议的字段限制之外，满足以下情况，该交易才被视为正确的**Wormhole**交易。
* `Property Name`字段不可以为NULL
* `Ecosystem`字段必须为1
* `Number of coins`创建的token总金额在范围之内
* `property precision`资产精度在范围之内

**Wormhole** 该类型交易协议字段如下


Filed | Type | Example|
----| ---| ---|
版本(Transaction version) |Transaction version|0
交易类型(Transaction type) |Transaction type|50
生态体系(Ecosystem)|Ecosystem|1
token精度(Property precision)|Property Precision|1
前tokenID(Previous Property ID) |Currency identifier|0
token类别(Property Category)|String null-terminated|""
token子类别(Property Subcategory)|String null-terminated|""
token名称(Property Name)|String null-terminated|"ludete"
token URL(Property URL)|String null-terminated|""
token描述数据(Property Data)|String null-terminated|""
创建的token数量(Number Properties)|Number of coins|10000


**Wormhole**的有效载荷数据：6a41087768630000003201000100000000746573740074657374320066697865642d746f6b656e0066697865642d746f6b656e006d7964617461000000000005f5e100

解释如下：
```
6a: OP_RETURN; 41: Wormhole协议的载荷数据长度； 
08776863: Wormhole协议的magic
0000: 版本
0032: 交易类型
01: 生态体系
0001: token 精度
00000000: 前TokenID
.... : 中间的为一些自定义数据
0000000005f5e100: 发行的token金额
```
示例交易：acec40a5ca1efed57a311b7dbb9324e753e7f5dd2395b3825163672b5edfc809


### 创建众筹Token(51)
创建一笔众筹Token，一旦该笔交易被**Wormhole**节点确认有效，该发送者的地址上将会拥有一笔活跃的众筹。

注意：当前的众筹只允许 使用WHC参与购买。

当下面的情况发生时，众筹将被永久关闭：
* 当块链的Tip块时间戳大于等于众筹截止时间`Deadline`时，该众筹将被永久关闭
* 众筹被手动强制关闭
* 众筹金额发售完毕

当众筹关闭时，未售完的众筹token会计入发行者的账户地址。

任何账户地址在指定时间只允许拥有一个活跃的众筹Token，避免众筹的参与者需要指定参加该账户的哪个众筹。

除**Wormhole**协议的字段限制之外，满足以下情况，该交易才被视为正确的**Wormhole**交易。
* `Property Name`字段不可以为NULL
* `Ecosystem`字段必须为1
* `Currency Identifier Desired`众筹的募集Token只允许是WHC(1)
* `Deadline`众筹的截止时间必须大于众筹的资产时间
* 众筹的购买者不可以是众筹的参与者

当其它账户发送交易参与该笔活跃众筹时，一旦参与众筹的交易被**Wormhole**节点确认有效，购买的众筹Token会立即计入参与方账户的可使用资金中，参与方可以将购买来的众筹Token进行转账，出售，或其它用途；同时，这笔参与众筹的资金(WHC)，会立即计入众筹发行方账户的可使用资金中，众筹发行方可以自由支配这些募集来的资金。

为了激励众筹参与者尽早参加众筹，可以通过设立初始化的早鸟奖励机制，来奖励早期的参与者。这个早鸟奖励随着众筹启动的时间线性降低，直至众筹截止时，早鸟激励将为0.

早鸟奖励的百分比计算公式如下：**((众筹的截止时间-交易所在的区块时间)/每周的时间秒数 ) * 早鸟激励**
示例如下
>两种Token兑换比例：1WHC=100HLT; 早鸟奖励机制为10%, 如购买1WHC:
众筹参与者的交易在众筹截止两周前参与，购买者会额外获得20%的Token， 120HLT；在众筹截止时间1.5周前参与，购买者会额外获得15%的Token，115HLT。

**Wormhole**客户端保证计入众筹参与者的Token数量加上发行者账户的Token数量不超过总的发行量。**当一笔参与众筹的交易，资金不足购买众筹Token的最小精度时，该笔众筹交易失败，同时资金会返还给参与方。**

**Wormhole** 该类型交易协议字段如下:


Filed | Type | Example|
----| ---| ---|
版本(Transaction version) |Transaction version|0
交易类型(Transaction type) |Transaction type|50
生态体系(Ecosystem)|Ecosystem|1
Token精度(Property Precision)|Precision|1
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
众筹的总量(totaltokens) | Number of coins| 100008


**Wormhole**的有效载荷数据：6a4c6c0877686300000033010002000000004372656174652043726f77642053616c6500426974436f696e00746573745f626974636f696e5f3000687474703a2f2f7777772e62616964752e636f6d00000000000100000002540be400000000005b8814800a00000000000bebc200 

解释如下：
```
6a: OP_RETURN; 4c: OP_PUSHDATA1; 6c: Wormhole协议的载荷数据长度； 
08776863: Wormhole协议的magic
0000: 版本
0033: 交易类型
01: 生态体系
0002: Token Precision
00000000:前TokenID
.... : 中间的为一些自定义数据
00000001:众筹募集的TokenID
00000002540be400: 兑换比例
000000005b881480: 众筹截止时间
0a: 早鸟激励的百分比
00: Undefined filed
000000000bebc200 : 众筹的总金额
```
示例交易：2dca56a9270c3f6c907fa73b99d1fc69033346ece31e3f166b67fa489189d78f

### 强制关闭众筹(53)
当众筹token处于活跃期时(即正在募集资金)，项目方基于token价值的考虑，需要提前结束众筹；可以通过构建一笔这样的交易，广播至块链。关闭众筹的操作不会对已经购买者的早鸟激励金造成任何影响。

除**Wormhole**协议的字段限制之外，存在以下情况的也为无效的**Wormhole**交易
* 关闭一个已经被关闭的众筹token
* 交易的发送者非众筹token的发行者

**Wormhole** 该类型交易协议字段如下:


Filed | Type | Example|
----| ---| ---|
版本(Transaction version) |Transaction version|0
交易类型(Transaction type) |Transaction type|53
tokenID(Property ID) |Currency identifier|9


**Wormhole**的有效载荷数据：6a0c087768630000003500000005
解释如下：
```
6a: OP_RETURN; 0c: Wormhole协议的载荷数据长度； 
08776863: Wormhole协议的magic
0000: 版本
0035: 交易类型
00000005: tokenID
```
示例交易: f40861a873a6fafadc9bc252b433268e780f232a53c4ed324ab5919d1cd23c01


### 创建可管理token(54)
创建一种可以被发行者管理的token，第一个交易输入(索引为0)的地址为该token的发行者，此时发行者的地址上可使用的金额为0，可以通过额外的两种交易类型来增发该token金额(增发token)，或销毁该token金额(销毁token).

除**Wormhole**协议的字段限制之外，必须满足以下情况，该交易才被视为正确的**Wormhole**交易。
* `Property Name`字段不可以为NULL
* `Ecosystem`字段必须为1

**Wormhole** 该类型交易协议字段如下:


Filed | Type | Example|
----| ---| --- | --- 
版本(Transaction version) |Transaction version|0|
交易类型(Transaction type) |Transaction type|50|
生态体系(Ecosystem)|Ecosystem|1|
token精度(Property Precision)|Property Precision|1|
冻结功能开关(Freeze Feature Switch) |Freeze Feature Switch|0|
token类别(Property Category)|String null-terminated|""|
token子类别(Property Subcategory)|String null-terminated|""|
token名称(Property Name)|String null-terminated|"ludete"|
token URL(Property URL)|String null-terminated|""|
token描述数据(Property Data)|String null-terminated|""|

**Wormhole**的有效载荷数据：6a3608776863000000360100010000000048656c6c6f00576f726c64006c7564657465007777772e6c75646574652e636f6d00796f6e6700

解释如下：
```
6a: OP_RETURN; 36: Wormhole协议的载荷数据长度； 
08776863: Wormhole协议的magic
0000: 版本
0036: 交易类型
01: 生态体系
0001: token precision
00000000: 冻结功能开关
.... : 下面的为一些自定义数据
```
示例交易: 7a42d544775aedd38c67fda3c20ba80af1385a9b65cb8e95e85024ec1732e3d4

**注意：**

自wormhole 0.1.1版本后Previous Property ID字段被赋予新的解释，代表资产冻结功能开关，默认值为0，表示关闭资产冻结功能；取值为1表示开启资产冻结功能。


### 增发token(55)
该交易类型用来配合可管理token，进行token金额的增发。在该增发交易被确认后，增发的token金额会被添加到接收者的可使用余额上。
注意：token创建后，初始金额为0； 允许的最大总金额为：(1 << 63) -1

除**Wormhole**协议的字段限制之外，存在以下情况的也为无效的**Wormhole**交易
* 增发的token类别不是可管理的类型的token
* 该笔交易的发送者非 token的发行者
* 累计token数量的总和超过了允许的最大金额。

**Wormhole** 该类型交易协议字段如下:


Filed | Type | Example|
----| ---| ---|
版本(Transaction version) |Transaction version|0
交易类型(Transaction type) |Transaction type|55
tokenID(Property ID) |Currency identifier|5
增发的数量(Number Properties)|Number of coins|1000
增发的信息(Memo)|String null-terminated| "reward"


**Wormhole**的有效载荷数据：6a1d08776863000000370000000c00000000000027107061792062696c6c00

解释如下：
```
6a: OP_RETURN; 1d: Wormhole协议的载荷数据长度； 
08776863: Wormhole协议的magic
0000: 版本
0037: 交易类型
0000000c:tokenID
0000000000002710:增发的金额
.... : 下面的为一些自定义数据
```
示例交易：1d0afab3383dfab315b7e3e2a5ba78d4ddb5481b8094843132e9134a4d589afc

### 销毁token(56)
该交易类型用来配合可管理token，进行token金额的销毁。在该销毁token交易被确认后，销毁的token金额会从发送者的地址上扣除，同时系统上该token的总金额也会降低。

除**Wormhole**协议的字段限制之外，存在以下情况的也为无效的**Wormhole**交易
* 销毁的token类别不是可管理的类型的token
* 该笔交易的发送者非 token的发行者
* 销毁的token数量超过了当前账户可以使用的token数量

**Wormhole** 该类型交易协议字段如下:


Filed | Type | Example|
----| ---| ---|
版本(Transaction version) |Transaction version|0
交易类型(Transaction type) |Transaction type|55
tokenID(Property ID) |Currency identifier|5
销毁的数量(Number Properties)|Number of coins|1000
销毁的信息(Memo)|String null-terminated| "spend"

**Wormhole**的有效载荷数据：6a1508776863000000380000000c0000000000000bb800

解释如下：
```
6a: OP_RETURN; 15: Wormhole协议的载荷数据长度； 
08776863: Wormhole协议的magic
0000: 版本
0038: 交易类型
0000000c:tokenID
0000000000000bb8:销毁的金额
.... : 下面的为一些自定义数据
```
示例交易: 74b8d70c1a8f2d9b95cea883f5a6adac1ffc421e3dea984ef79373bddb47364c


### 修改token的发行者(70)
该交易类型用来修改系统中已存在的token发行者。
对交易输入的规定：第一个交易输入为token的原始发行者
对交易输出的规定：最后一个交易输出为修改后的token发行者

除**Wormhole**协议的字段限制之外，存在以下情况的也为无效的**Wormhole**交易

*   不存在该笔资产
*   发送者，接收者，任何一个账户有活跃的众筹时，转移资产的发行者都会失败；
*   发送者非资产的原始发行者；
*   发送者，接收者地址相同；
*   接收者为空；
*   接收者为冻结地址；

**Wormhole** 该类型交易协议字段如下:


Filed | Type | Example|
----| ---| ---|
版本(Transaction version) |Transaction version|0
交易类型(Transaction type) |Transaction type|55
tokenID(Property ID) |Currency identifier|5

**Wormhole**的有效载荷数据：6a0c08776863000000460000000c

解释如下：
```
6a: OP_RETURN; 15: Wormhole协议的载荷数据长度； 
08776863: Wormhole协议的magic
0000: 版本
0038: 交易类型
0000000c:tokenID
```

示例交易：65fbd27bd1219b6c820aff9c43b2e175530cb0af0bbf14532fff132888af4906

### 冻结可管理资产(185)

该交易类型用来冻结指定地址下的可管理资产。

注意：该交易类型冻结地址下该资产所有余额，冻结数量字段该版本不生效。

对交易输入的规定：第一个交易输入为token的原始发行者

除**Wormhole**协议的字段限制之外，存在以下情况的也为无效的**Wormhole**交易

- 不存在该笔资产
- 资产不是可管理资产
- 冻结地址格式不是CashAddr地址
- 接受者地址为发行者地址

| Filed                        | Type                | Example                                    |
| ---------------------------- | ------------------- | ------------------------------------------ |
| 版本(Transaction version)    | Transaction version | 0                                          |
| 交易类型(Transaction type)   | Transaction type    | 185                                        |
| tokenID(Property ID)         | Currency identifier | 320                                        |
| 冻结数量(Number  Properties) | Number of coins     | 100                                        |
| 冻结地址(Address)            | Frozen Address      | qqpj0yu8w9ukg7x4h83xx7a4nj8f7mssh5dgn6flfu |

**Wormhole**的有效载荷数据：6a4708776863000000b90000014000000002540be400626368746573743a7171706a307975387739756b6737783468383378783761346e6a3866376d7373683564676e36666c667500

解释如下：

```
6a: OP_RETURN
47: Wormhole协议的载荷数据长度
08776863: Wormhole协议的magic
0000: 版本
00b9: 交易类型
00000140: tokenID
00000002540be400: 冻结金额，此处填充任何值都会冻结地址下指定token全部余额
626368746573743a7171706a307975387739756b6737783468383378783761346e6a3866376d7373683564676e36666c6675: 冻结地址
00: 地址字符串结束标记
```

示例交易：c6be48c6673f5d1f9c524317b274b445d0594e24fa882e89e2cb575f1b00aa7b

### 解冻可管理资产(186)

该交易类型用来解冻指定地址下的可管理资产。

注意：该交易类型冻结地址下该资产所有余额，冻结数量字段该版本不生效。

对交易输入的规定：第一个交易输入为token的原始发行者

除**Wormhole**协议的字段限制之外，存在以下情况的也为无效的**Wormhole**交易

- 不存在该笔资产
- 资产不是可管理资产
- 冻结地址格式不是CashAddr地址
- 接受者地址为发行者地址

| Filed                        | Type                | Example                                    |
| ---------------------------- | ------------------- | ------------------------------------------ |
| 版本(Transaction version)    | Transaction version | 0                                          |
| 交易类型(Transaction type)   | Transaction type    | 186                                        |
| tokenID(Property ID)         | Currency identifier | 320                                        |
| 冻结数量(Number  Properties) | Number of coins     | 100                                        |
| 冻结地址(Address)            | Frozen Address      | qqpj0yu8w9ukg7x4h83xx7a4nj8f7mssh5dgn6flfu |

**Wormhole**的有效载荷数据：

6a4708776863000000ba0000014000000002540be400626368746573743a7171706a307975387739756b6737783468383378783761346e6a3866376d7373683564676e36666c667500

解释如下：

```
6a: OP_RETURN
47: Wormhole协议的载荷数据长度
08776863: Wormhole协议的magic
0000: 版本
00b9: 交易类型
00000140: tokenID
00000002540be400: 解冻金额，此处填充任何值都会解冻地址下指定token全部余额
626368746573743a7171706a307975387739756b6737783468383378783761346e6a3866376d7373683564676e36666c6675: 解冻地址
00: 地址字符串结束标记
```

示例交易：78a35251b2f46157dc58e2769075f3a7abd8679af8eac04e8f0163e14144f63f



