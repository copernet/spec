
## Wormhole协议的解释
**Wormhole**协议是通过以**Bitcoin Cash**交易作为载体；使用**Bitcoin Cash**脚本中特殊的操作码`OP_RETURN`，来将**Wormhole**协议追加在该操作码后面。通俗来说：**Wormhole**交易是一种特殊的**Bitcoin Cash**交易，与**Bitcoin Cash**交易采用相同的安全、确认模型。

[**Wormhole**全节点客户端](https://github.com/copernet/Wormhole)是[bitcoin-abc核心客户端](https://github.com/Bitcoin-ABC/bitcoin-abc)的超集，具有**Bitcoin Cash**协议+**Wormhole**协议两种功能。

**Wormhole**交易在核心客户端的处理流程:
>
	1. 接收到区块后
	2. **Bitcoin Cash** 模块确认该交易的合法性
	3. **Wormhole**模块来确认**Wormhole**交易的合法性。

**Wormhole**协议采用的是账户模型，每个**Bitcoin Cash**地址是一个账户，每个账户地址可以含有多种Token。

## 协议字段定义
### Field: Transaction version
描述：交易类型的处理版本;每种交易类型的版本单调递增，且相互独立
字节：uint16_t;2 bytes

### Field: Transaction type
描述：**Wormhole**协议当前提供的功能
字节：uint16_t; 2 bytes
有效值：
>   0: Token转账
>   3: Token空投
>   4: 转移账户的所有Token至另一个账户地址
>   50: 创建固定数量的Token
>   51: 创建可众筹的Token
>   53: 强制关闭众筹
>   54: 创建可管理的Token
>   55: 增发Token
>   56: 销毁Token
>   68: 获取系统的基础货币
>   70: 修改Token的发行人

### Field: Ecosystem
描述：该Token所在的生态体系；
字节：uint8_t; 1 byte
有效值：当前有效值为1；

### Field: Number of coins
描述: 交易数据中受该字段影响的Token的数量，示例如下:
>   * 对于可分割的Token，该字段的值除以100,000,000标识影响的Token金额；(如1标识0.00000001LSH, 100000000标识1LSH)；**Wormhole**可分割Token的精度与**Bitcoin Cash**相同。
>   * 对于不可分割的Token，该字段的值就表示影响的Token金额；(如1标识1不可分割的Token)。
 
字节：int64_t; 8 bytes
有效值：1-9,223,372,036,854,775,807
>   * 对于可分割的Token金额范围：0.00000001 -  92,233,720,368.54775807
>   * 对于不可分割的Token金额范围：1 - 9,223,372,036,854,775,807

注意：当前**Wormhole**协议只支持不可分割的Token。

### Field: Property type
描述：标识**Wormhole**协议中创建的Token是否可以分割，以及创建的Token是否将取代或附加已存在的Token。
字节：uint16_t; 2 bytes
有效值：1, 2, 65, 66, 129, 130
>   * 1: 新的不可分割的Token
>   * 2: 新的可分割的Token
>   * 65: 创建不可分割的取代以前Token价值的新Token
>   * 66: 创建可分割的取代以前Token价值的新Token
>   * 129: 创建不可分割的追加以前Token价值的新Token
>   * 130: 创建可分割的追加以前Token价值的新Token

### Field: Currency identifier
描述：系统中的TokenID
字节：uint32_t; 4 bytes
有效值：
>   1, 3-2,147,483,647;
>   0:BCH; 1:WHC

### Field: Ecosystem
描述：Token所处的生态体系
字节：uint8_t; 1 byte
有效值：1

### Field: Integer-one byte
描述：用作乘数或作为其它计算的参数
字节：uint8_t; 1 byte
有效值：0-255

### Field: UTC Datetime
描述：Unix时间戳
字节：uint64_t; 8 bytes


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


### WHC的发行(68)
通过燃烧BCH来获取WHC，BCH与WHC的兑换比例是1:100；通过向指定的燃烧地址发送BCH可获取WHC。
燃烧地址：
>   主网: bitcoincash:qqqqqqqqqqqqqqqqqqqqqqqqqqqqqu08dsyxz98whc
>   测试网: bchtest:qqqqqqqqqqqqqqqqqqqqqqqqqqqqqdmwgvnjkt8whc
在主网中，1000个区块确认后，WHC会到账；在测试网中，3个区块确认后，WHC会到账。

1BCH = 100WHC; 1WHC = 100,000,000C;

通过燃烧BCH获取WHC的交易规范:
```
对交易输入的规定：`**Wormhole**引擎默认第一个交易输入(即索引为0)作为获取WHC的账户地址`
对交易输出的规定：`**Wormhole**引擎默认：第一个交易输出必须为燃烧输出(即索引为0)；第二个输出为**Wormhole**协议的数据；第三个输出为多余的BCH赎回地址输出(这个输出可以忽略)`
对燃烧金额的规定：`最少燃烧1BCH`。
交易规范中主要涉及：**交易输入、输出的顺序，燃烧金额，**Wormhole**载荷数据**
```
**Wormhole** 该类型交易协议字段如下
Filed | Type | Example|
----| ---| ---|
版本(Transaction version) | Transaction version | 0
交易类型(Transaction type) | Transaction type | 68

除**Wormhole**协议的字段限制之外，存在以下情况的也为无效的**Wormhole**交易
>   * 燃烧BCH的输出不在索引为0的位置
>   * 索引为1的位置不是**Wormhole**的燃烧协议数据
>   * 燃烧的BCH金额小于1BCH

操作示例如下：如果想燃烧1BCH，获取相应的WHC，两种方式。

* 方案一: 
`Wormholed-cli whc_burnbchgetwhc 1 ""`

* 方案二: 用户指定交易输入，作为接受WHC的地址。
添加交易输入：`Wormholed-cli whc_createrawtx_input "" "address1" index` (这步用户指定接收WHC的地址:address1)
添加交易输出，向指定的燃烧地址打币: `Wormholed-cli whc_createrawtx_reference "rawtx" "address2" 1` (燃烧地址:address2)
创建**Wormhole**载荷数据: `Wormholed-cli whc_createpayload_burnbch`
添加交易输出，将**Wormhole**载荷数据添加进交易输出: `Wormholed-cli whc_createrawtx_opreturn "rawtx" "payload" `
添加交易输出，进行找零(可选): `Wormholed-cli whc_createrawtx_reference "rawtx" "address3" 0.5` (找零地址:address3)
对创建的交易进行签名: `Wormholed-cli signrawtransaction "rawtx"`
发送交易: `Wormholed-cli sendrawtransaction "hextx"`
返回值为：生成的交易哈希

**Wormhole**的有效载荷数据：6a080877686300000044
解释如下：
>   6a: OP_RETURN; 08: **Wormhole**协议的数据长度； 
>   08776863: **Wormhole**协议的magic
>   0000: 版本
>   0044: 交易类型

示例交易: 53d2d2701bdca225ae4486e72854369a09499a0555c22ef1936c02924fe901cb

### Token转账(0)
从指定的账户地址向另一个账户地址**转移**指定**金额**的**某种Token**。
注意：**最后一个输出必须为Token的接收者，第一个输入必须为Token的发送者**

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
转账金额(Amount to transfer)|Number of Coins|1000(1000C)

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

### Token空投(3)
将一种Token(ID1)的指定金额**空投**给某种Token(ID2)的持有人(除了发送者)，所有接受空投的地址获取的金额(ID1)按所含有的Token(ID2)比例进行分配。
注意：**第一个输入必须为空投的发送者**

空投会收取一定的燃料费，以WHC计价，每场空投消耗的手续费为：接收空投的人数*系统单价(1C);

**Wormhole** 该类型交易协议字段如下
Filed | Type | Example|
----| ---| ---|
版本(Transaction version) |Transaction version | 0
交易类型(Transaction type) |Transaction type  | 3
空投TokenID(Property ID) | Currency identifier | 1
空投金额(Amount to transfer)|Amount to transfer | 15000000000
目的TokenID(Property ID) |Currency identifier | 1

除**Wormhole**协议的字段限制之外，存在以下情况的也为无效的**Wormhole**交易
>   * 发送者的地址上可使用的金额小于空投的金额
>   * 指定的Token不存在，或者TokenID为0
>   * 发送者的地址上没有足够的WHC支付燃料费
>   * 发送者地址上含有所有的目的Token

操作示例如下：如想转账1WHC两种方式。

* 方案一：
`Wormholed-cli whc_sendsto fromaddress propertyid amount redeemaddress distributionproperty`
redeemaddress: BCH 的找零地址，默认为发送者地址
distributionproperty: 空投参与者的TokenID，默认为空投的TokenID

* 方案二：
添加交易输入：`Wormholed-cli whc_createrawtx_input "" txid index `
创建空投的载荷数据：` Wormholed-cli whc_createpayload_sto propertyid amount distributionproperty `
添加交易输出，将创建的**Wormhole**载荷数据添加进交易输出：`Wormholed-cli whc_createrawtx_opreturn "rawtx" "payload"`
添加交易输出，进行找零：`Wormholed-cli whc_createrawtx_reference "rawtx" "destination"  amount`
对创建的交易进行签名：`Wormholed-cli signrawtransaction "rawtx" `
发送交易：`Wormholed-cli sendrawtransaction "tx" `


**Wormhole**的有效载荷数据：6a18087768630000000300000001000000037e11d600
解释如下：
>   6a: OP_RETURN; 18: **Wormhole**协议的数据长度； 
>   08776863: **Wormhole**协议的magic
>   0000: 版本
>   0003: 交易类型
>   00000001: TokenID
>   000000037e11d600: 空投金额（1.5WHC）
>   00000001: TokenID

示例交易：9bb8eb45e5b3ba4059c38409a225216f3aaae990f07d2541f1c09571862b99d2

### 转移账户的所有Token(4)
转移一个账户地址(A)的所有Token至另一个账户地址(B)，此处资产不包含BCH.
注意：**最后一个输出必须为Token的接收者，第一个输入必须为Token的发送者**

**Wormhole** 该类型交易协议字段如下
Filed | Type | Example|
----| ---| ---|
版本(Transaction version) |Transaction version | 0
交易类型(Transaction type) |Transaction type  | 4
生态体系(Ecosystem)| Ecosystem |1

操作示例如下：如想转账1WHC两种方式。

* 方案一：
`Wormholed-cli whc_sendall fromaddress toaddress ecosystem (redeemaddress) (referenceamount)`
redeemaddress: 赎回BCH的地址，默认为发送者的地址；
referenceamount: 给Token接收者的输出的BCH金额，默认为最小输出金额

* 方案二：
添加交易输入：` Wormholed-cli whc_createrawtx_input "" "txid" index`
生成**Wormhole**的载荷数据：`Wormholed-cli whc_createpayload_sendall ecosystem `
添加交易输出，将创建的**Wormhole**载荷数据添加进交易输出：`Wormholed-cli whc_createrawtx_opreturn "rawtx" "payload"`
添加交易输出，进行找零：`Wormholed-cli whc_createrawtx_reference "rawtx" "destination"  amount`
添加交易输出，将所有Token转入该地址：`Wormholed-cli whc_createrawtx_reference "rawtx" "destination"  amount`
对创建的交易进行签名：`Wormholed-cli signrawtransaction "rawtx"`
发送交易：`Wormholed-cli sendrawtransaction "tx"`

**Wormhole**的有效载荷数据：6a09087768630000000401
解释如下：
>   6a: OP_RETURN; 09: **Wormhole**协议的数据长度； 
>   08776863: **Wormhole**协议的magic
>   0000: 版本
>   0004: 交易类型
>   01: 生态体系

示例交易：8f4a11bb724139a43494b08da35beac1a53c34aec13eab22d188540f5cc0c164


### 创建固定数量的Token(50)
创建指定数量的Token，一旦该交易被**Wormhole**有效确认，创建者地址上会含有这些数量的Token。

除**Wormhole**协议的字段限制之外，满足以下情况，该交易才被视为正确的**Wormhole**交易。
>   * 当`Property Type`字段标识新Token时，`Previous Property ID`字段必须为0
>   * `Property Name`字段不可以为NULL
>   * `Ecosystem`字段必须为1
>   * `Property Type`字段必须为1

**Wormhole** 该类型交易协议字段如下
Filed | Type | Example|
----| ---| ---|
版本(Transaction version) |Transaction version|0
交易类型(Transaction type) |Transaction type|50
生态体系(Ecosystem)|Ecosystem|1
Token类型(Property Type)|Property Type|1
前TokenID(Previous Property ID) |Currency identifier|0
Token类别(Property Category)|String null-terminated|""
Token子类别(Property Subcategory)|String null-terminated|""
Token名称(Property Name)|String null-terminated|"ludete"
Token URL(Property URL)|String null-terminated|""
Token描述数据(Property Data)|String null-terminated|""
**Wormhole**(Number Properties)|Number of coins|10000

操作示例如下：如想转账1WHC两种方式。

* 方案一：
`Wormholed-cli whc_sendissuancefixed fromaddress ecosystem type previousid category subcategory name url data amount`

* 方案二：
添加交易输入：`Wormholed-cli whc_createrawtx_input "" txid index`
生成创建固定Token的载荷数据：`Wormholed-cli whc_createpayload_issuancefixed ecosystem type previousid category subcategory name url data amount`
添加交易输出，将创建的**Wormhole**载荷数据添加进交易输出：
`Wormholed-cli whc_createrawtx_opreturn "rawtx" "payload"`
添加交易输出，进行找零：`Wormholed-cli whc_createrawtx_reference "rawtx" "destination"  amount`
对创建的交易进行签名：`Wormholed-cli signrawtransaction "rawtx"`
发送交易：`Wormholed-cli sendrawtransaction "tx"`

**Wormhole**的有效载荷数据：6a41087768630000003201000100000000746573740074657374320066697865642d746f6b656e0066697865642d746f6b656e006d7964617461000000000005f5e100

解释如下：
>   6a: OP_RETURN; 41: **Wormhole**协议的数据长度； 
>   08776863: **Wormhole**协议的magic
>   0000: 版本
>   0032: 交易类型
>   01: 生态体系
>   0001: Token类别
>   .... : 中间的为一些自定义数据
>   0000000005f5e100: 发行的Token金额

示例交易：bf8a2e94eba3ee0c42f14fb2e3a91d2b42e0f11222db9ba7804054b614f3ea16


### 创建众筹Token(51)
创建一笔众筹Token，一旦该笔交易被**Wormhole**节点确认，该发送者的地址上将会拥有一种Token。
注意：当前的众筹只允许 使用WHC参与。众筹的总金额是固定的，int64的最大值：4611686018427387904

当下面的情况发生时，众筹将被永久关闭：
>   * 当块链的Tip块时间戳大于等于众筹截止时间`Deadline`时，该众筹将被永久关闭
>   * 众筹被手动强制关闭
>   * 众筹金额发售完毕

任何账户地址在指定时间只允许拥有一个活跃的众筹Token，避免众筹的参与者需要指定参加该账户的哪个众筹。
计入众筹参与方与众筹发行人的众筹Token将会被立即添加到各自地址的可用资产中，这些可用众筹可以被各自的账户花费或以其它方式使用。募集的资金将被添加到众筹发行者的可用资产余额中，并允许该地址花费或以其它方式使用这些资产。

为了激励众筹参与者尽早参加众筹，可以通过设立初始化的早鸟奖励机制，来奖励早期的参与者。这个早鸟奖励随着众筹启动的时间线性降低，直至众筹截止时，早鸟激励将为0.

早鸟奖励的百分比计算公式如下：**((众筹的截止时间-交易所在的区块时间)/每周的时间秒数 + (众筹的截止时间-交易所在的区块时间)%每周的时间秒数) * 早鸟激励**
示例如下
>两种Token兑换比例：1WHC=100HLT; 早鸟奖励机制为10%
众筹参与者的交易在众筹截止两周前参与，购买者会获得20%的额外Token；在众筹截止时间1.5周前参与，购买者会获得15%的额外Token，如购买1WHC，会获得115HLT。

**Wormhole**客户端保证计入参与者的Token数量加上发行者账户的Token数量不超过总的发行量。如果最后一笔众筹购买时，总量超出发行的总量，协议会减少最后一笔众筹计入参与者与发行者相应的Token数量，确保这笔众筹的购买交易只能够购买剩余的未发行的数量的Token。

`给发行者的百分比(Percentage for issuer)`:每笔参与众筹交易，按照该比例，向发行者地址的可用余额计入相应数量的Token.
共识如下：该笔众筹购买的Token数量 * ((Percentage for issuer) / 100)

**Wormhole** 该类型交易协议字段如下
Filed | Type | Example|
----| ---| ---|
版本(Transaction version) |Transaction version|0
交易类型(Transaction type) |Transaction type|50
生态体系(Ecosystem)|Ecosystem|1
Token类型(Property Type)|Property Type|1
前TokenID(Previous Property ID) |Currency identifier|0
Token类别(Property Category)|String null-terminated|""
Token子类别(Property Subcategory)|String null-terminated|""
Token名称(Property Name)|String null-terminated|"ludete"
Token URL(Property URL)|String null-terminated|""
Token描述数据(Property Data)|String null-terminated|""
众筹募集的TokenID(Currency Identifier Desired) |Currency identifier|1
兑换比例(Number Properties per Unit Invested)|Number of Coins|100(募集Token:众筹Token)
众筹截止时间(Deadline)|Deadline|Unix时间戳
早鸟激励的百分比(Early Bird Bonus %/Week	)|Integer one-byte | 10
给发行者的百分比(Percentage for issuer)|Integer one-byte | 12

除**Wormhole**协议的字段限制之外，满足以下情况，该交易才被视为正确的**Wormhole**交易。
>   * 当`Property Type`字段标识新Token时，`Previous Property ID`字段必须为0
>   * `Property Name`字段不可以为NULL
>   * `Ecosystem`字段必须为1
>   * `Property Type`字段必须为1
>   * `Currency Identifier Desired`众筹的募集Token只允许是WHC(1)
>   * `Deadline`众筹的截止时间必须大于众筹的发起时间

操作示例如下：如想转账1WHC两种方式。

* 方案一：
`Wormholed-cli whc_sendissuancecrowdsale fromaddress ecosystem type previousid category subcategory name url data propertyiddesired Tokensperunit deadline earlybonus issuerpercentage`

* 方案二：
添加交易输入：`Wormholed-cli whc_createrawtx_input "" txid index`
生成创建众筹Token的载荷数据：`Wormholed-cli whc_createpayload_issuancecrowdsale ecosystem type previousid category subcategory name url data propertyiddesired Tokensperunit deadline earlybonus issuerpercentage`
添加交易输出，将创建的**Wormhole**载荷数据添加进交易输出：
`Wormholed-cli whc_createrawtx_opreturn "rawtx" "payload"`
添加交易输出，进行找零：`Wormholed-cli whc_createrawtx_reference "rawtx" "destination"  amount`
对创建的交易进行签名：`Wormholed-cli signrawtransaction "rawtx"`
发送交易：`Wormholed-cli sendrawtransaction "tx"`

**Wormhole**的有效载荷数据：6a4c5f087768630000003301000100000000416374697669746965730041637469766974696573206f66006f6d6e696361736800687474703a2f2f7777772e676f6f676c652e636100222200 00000001 00000002540be400 000000005b574d00 0a0a

解释如下：
>   6a: OP_RETURN; 41: **Wormhole**协议的数据长度； 
>   08776863: **Wormhole**协议的magic
>   0000: 版本
>   0033: 交易类型
>   01: 生态体系
>   0001: Token类别
>   00000000:前TokenID
>   .... : 中间的为一些自定义数据
>   0000000005f5e100: 发行的Token金额
>   00000001:众筹募集的TokenID
>   00000002540be400: 兑换比例
>   000000005b574d00: 众筹截止时间
>   0a: 早鸟激励的百分比
>   0a: 给发行者的百分比

示例交易：4037086b5d1c16e97bc791f242db4996241202a2ac3e9700b6098547b552ffc2

### 强制关闭众筹(53)
当在众筹Token处于活跃期时(即正在募集资金)，项目方基于Token价值的考虑，需要提前结束众筹；可以通过构建一笔这样的交易，广播至块链。关闭众筹的操作不会对已经购买者的早鸟激励金造成任何影响。

除**Wormhole**协议的字段限制之外，存在以下情况的也为无效的**Wormhole**交易
>   * 关闭一个已经被关闭的众筹Token
>   * 交易的发送者非众筹Token的发行者

**Wormhole** 该类型交易协议字段如下
Filed | Type | Example|
----| ---| ---|
版本(Transaction version) |Transaction version|0
交易类型(Transaction type) |Transaction type|53
TokenID(Property ID) |Currency identifier|9

注意：**参与已被关闭的众筹属于无效投资，参与者不会获取任何众筹Token，且参与众筹的投资金额会计入项目方地址，需要参与者与项目方沟通，赎回这笔投资资金**




### 创建可管理Token(54)
创建一种可以被发行者管理的Token，第一个交易输入(索引为0)的地址为该Token的发行者，此时发行者的地址上可使用的金额为0，可以通过额外的两种交易类型来增发该Token金额(增发Token)，或销毁该Token金额(销毁Token).

除**Wormhole**协议的字段限制之外，必须满足以下情况，该交易才被视为正确的**Wormhole**交易。
>   * 当`Token类型`字段标识新Token时，`前TokenID`字段必须为0
>   * `Property Name`字段不可以为NULL
>   * `Ecosystem`字段必须为1
>   * `Property Type`字段必须为1

**Wormhole** 该类型交易协议字段如下
Filed | Type | Example|
----| ---| ---|
版本(Transaction version) |Transaction version|0
交易类型(Transaction type) |Transaction type|50
生态体系(Ecosystem)|Ecosystem|1
Token类型(Property Type)|Property Type|1
前TokenID(Previous Property ID) |Currency identifier|0
Token类别(Property Category)|String null-terminated|""
Token子类别(Property Subcategory)|String null-terminated|""
Token名称(Property Name)|String null-terminated|"ludete"
Token URL(Property URL)|String null-terminated|""
Token描述数据(Property Data)|String null-terminated|""


操作示例如下：如想转账1WHC两种方式。

* 方案一：
`Wormholed-cli whc_sendissuancemanaged fromaddress ecosystem type previousid category subcategory name url data`

* 方案二：
添加交易输入：`Wormholed-cli whc_createrawtx_input "" txid index`
生成创建管理Token的载荷数据：`Wormholed-cli whc_createpayload_issuancemanaged ecosystem type previousid category subcategory name url data propertyiddesired Tokensperunit deadline earlybonus issuerpercentage`
添加交易输出，将创建的**Wormhole**载荷数据添加进交易输出：
`Wormholed-cli whc_createrawtx_opreturn "rawtx" "payload"`
添加交易输出，进行找零：`Wormholed-cli whc_createrawtx_reference "rawtx" "destination"  amount`
对创建的交易进行签名：`Wormholed-cli signrawtransaction "rawtx"`
发送交易：`Wormholed-cli sendrawtransaction "tx"`

**Wormhole**的有效载荷数据：6a3608776863000000360100010000000048656c6c6f00576f726c64006c7564657465007777772e6c75646574652e636f6d00796f6e6700

解释如下：
>   6a: OP_RETURN; 36: **Wormhole**协议的数据长度； 
>   08776863: **Wormhole**协议的magic
>   0000: 版本
>   0036: 交易类型
>   01: 生态体系
>   0001: Token类别
>   00000000:前TokenID
>   .... : 下面的为一些自定义数据


示例交易：b85ffb2d38339b4432cd8dfe50861e05376665f1ffe08c4824dea934aadb20e1


### 增发Token(55)
该交易类型用来配合可管理Token，进行Token金额的增发。在该增发交易被确认后，增发的Token金额会被添加到接收者的可使用余额上。
注意：Token创建后，初始金额为0； 允许的最大总金额为：(1 << 63) -1

除**Wormhole**协议的字段限制之外，存在以下情况的也为无效的**Wormhole**交易
>   * 增发的Token类别不是可管理的类型的Token
>   * 该笔交易的发送者非 Token的发行者
>   * 累计Token数量的总和超过了允许的最大金额。


**Wormhole** 该类型交易协议字段如下
Filed | Type | Example|
----| ---| ---|
版本(Transaction version) |Transaction version|0
交易类型(Transaction type) |Transaction type|55
TokenID(Property ID) |Currency identifier|5
增发的数量(Number Properties)|Number of coins|1000
增发的信息(Memo)|String null-terminated| "reward"


操作示例如下：

* 方案一：
`Wormholed-cli whc_sendgrant fromaddress toaddress propertyid amount  ("data")`

* 方案二：
添加交易输入：`Wormholed-cli whc_createrawtx_input "" txid index`
生成创建管理Token的载荷数据：`Wormholed-cli whc_createpayload_grant propertyid amount ("data")`
添加交易输出，将创建的**Wormhole**载荷数据添加进交易输出：
`Wormholed-cli whc_createrawtx_opreturn "rawtx" "payload"`
添加交易输出，进行找零：`Wormholed-cli whc_createrawtx_reference "rawtx" "destination"  amount`
对创建的交易进行签名：`Wormholed-cli signrawtransaction "rawtx"`
发送交易：`Wormholed-cli sendrawtransaction "tx"`


**Wormhole**的有效载荷数据：6a1d08776863000000370000000c00000000000027107061792062696c6c00

解释如下：
>   6a: OP_RETURN; 1d: **Wormhole**协议的数据长度； 
>   08776863: **Wormhole**协议的magic
>   0000: 版本
>   0037: 交易类型
>   01: 生态体系
>   0001: Token类别
>   0000000c:TokenID
>   0000000000002710:增发的金额
>   .... : 下面的为一些自定义数据

示例交易：6a1d08776863000000370000000c00000000000027107061792062696c6c00

### 销毁Token
该交易类型用来配合可管理Token，进行Token金额的销毁。在该销毁Token交易被确认后，销毁的Token金额会从发送者的地址上扣除，同时系统上该Token的总金额也会降低。

除**Wormhole**协议的字段限制之外，存在以下情况的也为无效的**Wormhole**交易
>   * 销毁的Token类别不是可管理的类型的Token
>   * 该笔交易的发送者非 Token的发行者
>   * 销毁的Token数量超过了当前账户可以使用的Token数量

**Wormhole** 该类型交易协议字段如下
Filed | Type | Example|
----| ---| ---|
版本(Transaction version) |Transaction version|0
交易类型(Transaction type) |Transaction type|55
TokenID(Property ID) |Currency identifier|5
销毁的数量(Number Properties)|Number of coins|1000
销毁的信息(Memo)|String null-terminated| "spend"

操作示例如下：

* 方案一：
`Wormholed-cli whc_sendgrant fromaddress toaddress propertyid amount  ("data")`

* 方案二：
添加交易输入：`Wormholed-cli whc_createrawtx_input "" txid index`
生成创建管理Token的载荷数据：`Wormholed-cli whc_createpayload_revoke propertyid amount ("data")`
添加交易输出，将创建的**Wormhole**载荷数据添加进交易输出：
`Wormholed-cli whc_createrawtx_opreturn "rawtx" "payload"`
添加交易输出，进行找零：`Wormholed-cli whc_createrawtx_reference "rawtx" "destination"  amount`
对创建的交易进行签名：`Wormholed-cli signrawtransaction "rawtx"`
发送交易：`Wormholed-cli sendrawtransaction "tx"`

**Wormhole**的有效载荷数据：6a1508776863000000380000000c0000000000000bb800

解释如下：
>   6a: OP_RETURN; 15: **Wormhole**协议的数据长度； 
>   08776863: **Wormhole**协议的magic
>   0000: 版本
>   0038: 交易类型
>   01: 生态体系
>   0001: Token类别
>   0000000c:TokenID
>   0000000000000bb8:销毁的金额
>   .... : 下面的为一些自定义数据


示例交易：d5966fd7568b29b19ba5400d94e4fa8adbc91040fd0261f1209db163518a0327


### 修改Token的发行者(70)
该交易类型用来修改系统中已存在Token的发行者。
对交易输入的规定：第一个交易输入为Token的原始发行者
对交易输出的规定：最后一个交易输出为修改后的Token发行者

除**Wormhole**协议的字段限制之外，存在以下情况的也为无效的**Wormhole**交易
>   * 交易的发送者非原始Token的发行者
>   * 没有指定新的发行者地址。

**Wormhole** 该类型交易协议字段如下
Filed | Type | Example|
----| ---| ---|
版本(Transaction version) |Transaction version|0
交易类型(Transaction type) |Transaction type|55
TokenID(Property ID) |Currency identifier|5


操作示例如下：

* 方案一：
`Wormholed-cli whc_sendchangeissuer fromaddress toaddress propertyid `

* 方案二：
添加交易输入：`Wormholed-cli whc_createrawtx_input "" txid index`
生成**Wormhole**的载荷数据：`Wormholed-cli whc_createpayload_changeissuer propertyid `
添加交易输出，将创建的**Wormhole**载荷数据添加进交易输出：
`Wormholed-cli whc_createrawtx_opreturn "rawtx" "payload"`
添加交易输出，进行找零：`Wormholed-cli whc_createrawtx_reference "rawtx" "destination"  amount`(这步可以忽略)
添加交易输出，作为Token新的发行者：`Wormholed-cli whc_createrawtx_reference "rawtx" "destination"  amount`
对创建的交易进行签名：`Wormholed-cli signrawtransaction "rawtx"`
发送交易：`Wormholed-cli sendrawtransaction "tx"`

**Wormhole**的有效载荷数据：6a0c08776863000000460000000c

解释如下：
>   6a: OP_RETURN; 15: **Wormhole**协议的数据长度； 
>   08776863: **Wormhole**协议的magic
>   0000: 版本
>   0038: 交易类型
>   0000000c:TokenID


示例交易：ce6877f4da9e627741cf087aa777917f5ecf5fb8ff47037b211140b68155d9eb

