## RPC 的业务整理
name        |  callfunction | param | return value  |
---- |  ---| ---- | --- | 
`listblocktransactions_MP` | `omni_listblocktransactions` | 指定的块高度 | 列出指定块号的区块，包含的所有omni交易的哈希列表
`omni_listpendingtransactions` | `omni_listpendingtransactions` | 可选的过滤地址 |  列出系统中还在交易池的所有/过滤地址相关的omni详细信息列表
`omni_getpayload` | `omni_getpayload` | 交易哈希 | 返回指定交易的omni数据结果，对于非omni交易，抛异常
`getsto_MP` | `omni_getsto` | 指定的空投交易哈希 |   |
`getcrowdsale_MP` | `omni_getcrowdsale` | 参1：众筹资产ID，参2：可选，是否获取参与者的详细信息|  返回指定众筹资产的详细信息；对于非众筹的资产，直接报错|
`getgrants_MP` | `omni_getgrants` | 管理资产的ID | 返回管理资产的所有信息，包括增发和销毁的历史数据。
`getproperty_MP` | `omni_getproperty` | 资产ID | 返回资产的所有基础信息 |
`omni_createpayload_issuancefixed` | `omni_createpayload_issuancefixed` | 参1:生态体系；参2: Token类型；参3: 是否为派生资产; 参4: 资产的自定义类别; 参5: 资产的子类别；参6: 资产的名称；参7: 资产的官网；参8: 新资产的描述数据；参9：发行的资产总金额| omni协议序列化后的数据 |
`omni_createpayload_issuancecrowdsale` | `omni_createpayload_issuancecrowdsale` | 前8个参数，与上述相同；参9: 期望参与者使用的Token；参10: 期望资产与众筹资产的兑换比例；参11: 众筹的截止时间；参12: 参与者的早鸟奖励； 参13: 按参与者购买的数量，授予发行者代币的比例| omni协议序列化后的数据 |
`omni_createpayload_issuancemanaged`|`omni_createpayload_issuancemanaged`|该RPC8个参数，与上述前8个参数意义相同| omni协议序列化后的数据 |
`omni_createpayload_burnbch` |  `omni_createpayload_burnbch` |  无 | 创建基础货币的载荷数据
`omni_createpayload_simplesend` | `omni_createpayload_simplesend` | 参1: 资产ID； 参2: 转账的Token数量| 创建发送货币的omni载荷数据|
`omni_createpayload_grant` | `omni_createpayload_grant` | 参1:增发的资产ID；参2:增发的金额；参3:增发的原因(可选) | omni协议序列化后的数据
`omni_createpayload_revoke` | `omni_createpayload_revoke` | 参1:销毁的资产ID；参2:销毁的金额；参3:销毁的原因(可选) | omni协议序列化后的数据
`omni_createpayload_sto`| `omni_createpayload_sto` | 参1: 将要进行空投的资产ID；参2: 进行空投的金额；参3: 给哪种资产的持有人进行空投| omni协议序列化后的数据|
`omni_createpayload_sendall` | `omni_createpayload_sendall`|参数1:资产的生态 |omni协议序列化后的数据 |
`omni_createpayload_changeissuer` | `omni_createpayload_changeissuer` |参1:资产ID | omni协议序列化后的数据|
`omni_createrawtx_input` |`omni_createrawtx_input` |向参1所在的交易添加一个交易输入；参1: 16进制的未签名交易(可以传空字符串); 参2:txid; 参3:输出索引 | 添加交易输入后的未签名交易的16进制数据|
`omni_createrawtx_reference` | `omni_createrawtx_reference` | 向参1所在的交易添加一个输出；参1: 16进制的未签名交易(可以传空字符串)；参2: 输出的目的地址；参3: 指定的输出金额(可选)| 添加交易输出后的未签名交易的16进制数据|
`omni_createrawtx_opreturn` | `omni_createrawtx_opreturn` | 添加一个omni C类交易输出至参1所在的交易；参1: 16进制的未签名交易(可以传空字符串)； 参2: 将要添加至输出的C类载荷数据 | 添加交易输出后的未签名交易的16进制数据|
`omni_createrawtx_change` | `omni_createrawtx_change` | 添加一个输出，一般用来添加一个找零输出；参1:未签名的交易；参2:该交易所使用的交易输入；参3: 添加输出的锁定脚本；参4: 参1交易期望的交易费；参5: 该找零输出期望插入的位置；| 添加交易输出后的未签名交易的16进制数据|
`whc_burnbchgetwhc`|`whc_burnbchgetwhc`|参1:燃烧的BCH金额；参2:多余的赎回资金地址(可选)|生成的获取WHC的交易哈希|
`whc_send`|`whc_send`|参1:发送者地址；参2:接收者地址；参3:要发送的资产ID；参4:发送的Token数量；参5:多余的sh|生成的转账交易哈希|
`whc_sendall`|`whc_sendall`|参1:发送者地址；参2:接收者地址；参3:生态体系(强制为1)|生成的转账交易哈希|
`whc_sendrawtx`| `whc_sendrawtx`|**注意:本RPC不可以用来操作燃烧BCH获取WHC**。参1:发送者地址；参2:wormhole协议载荷数据；参3:接收者地址；参4:赎回BCH的地址；参5:给接收者地址的金额| 生成的wormhole交易的ID|
`whc_sendissuancecrowdsale`|`whc_sendissuancecrowdsale`|与上述`whc_createpayload_issuancecrowdsale`参数列表相同|生成的创建众筹资产的交易哈希|
`whc_sendissuancefixed`|`whc_sendissuancefixed`|与上述`whc_createpayload_issuancefixed`参数列表相同|生成的创建固定资产的交易哈希|
`whc_sendissuancemanaged`|`whc_sendissuancemanaged`|与上述`whc_createpayload_issuancefixed`参数列表相同| 生成的创建可管理资产的交易哈希|
`whc_sendchangeissuer`|`whc_sendchangeissuer`|参1:原资产的发行者；参2:将拥有该资产的所有者；参3:转移的资产ID|生成的转移资产所有者的交易哈希|
`whc_sendclosecrowdsale`|`whc_sendclosecrowdsale`|参1:资产的发行者； 参2:资产ID|生成的关闭众筹的交易哈希|
`whc_sendgrant`|`whc_sendgrant`|参1:资产的发行者；参2:给指定的地址增发金额；参3:资产ID；参4:增发的金额；参5:增发原因(可选)|生成的增发交易的哈希|
`whc_sendrevoke`|`whc_sendrevoke`|参1:资产的发行者；参2:资产ID；参3:销毁的金额；参4:销毁原因(可选)|生成的销毁资金交易哈希|
`whc_sendsto`|`whc_sendsto`|参1:空投发行人的地址；参2:空投的资产ID；参3:空投的金额；参4:赎回的BCH的地址(可选)；参5:给某种资产的持有人进行空投(可选)|生成的空投交易哈希|

## RPC的调用流程
### 燃烧BCH，获取基础货币RPC调用流程
1. 添加交易输入：   `omnicashd-cli omni_createrawtx_input `
2. 创建输出，向指定的燃烧地址打币： `omnicashd-cli omni_createrawtx_reference`
3. 创建燃烧BCH的omni载荷数据：  `omnicashd-cli omni_createpayload_burnbch`
4. 创建输出，将创建的omni载荷数据添加进交易输出： `omnicashd-cli omni_createrawtx_opreturn`
5. 创建输出，进行找零： `omnicashd-cli omni_createrawtx_reference`  (这步可以省略，多余的金额会全部作为交易费)
6. 对创建的交易进行签名：`omnicashd-cli signrawtransaction`
7. 发送交易：`omnicashd-cli sendrawtransaction`

### 转币
1. 添加交易输入：   `omnicashd-cli omni_createrawtx_input `
2. 创建发送Token的载荷数据：    `omnicashd-cli omni_createpayload_simplesend`
3. 创建交易输出，将创建的omni载荷数据添加进交易输出：   `omnicashd-cli omni_createrawtx_opreturn`
4. 创建输出，进行找零：`omnicashd-cli omni_createrawtx_reference`
5. 创建输出，将Token转入该地址：`omnicashd-cli omni_createrawtx_reference`
6. 对创建的交易进行签名：`omnicashd-cli signrawtransaction`
7. 发送交易：`omnicashd-cli sendrawtransaction`

### 转移所有的资产
1. 添加交易输入：   `omnicashd-cli omni_createrawtx_input `
2. 创建发送Token的载荷数据：    `omnicashd-cli omni_createpayload_sendall`
3. 创建交易输出，将创建的omni载荷数据添加进交易输出：   `omnicashd-cli omni_createrawtx_opreturn`
4. 创建输出，进行找零：`omnicashd-cli omni_createrawtx_reference`（这步可以省略）
5. 创建输出，将Token转入该地址：`omnicashd-cli omni_createrawtx_reference`
6. 对创建的交易进行签名：`omnicashd-cli signrawtransaction`
7. 发送交易：`omnicashd-cli sendrawtransaction`

### 创建固定数量的Token资产
1. 添加交易输入：   `omnicashd-cli omni_createrawtx_input `
2. 生成创建固定资产的载荷数据： `omnicashd-cli omni_createpayload_issuancefixed`
3. 创建交易输出，将创建的omni载荷数据添加进交易输出：   `omnicashd-cli omni_createrawtx_opreturn`
4. 创建输出，进行找零：`omnicashd-cli omni_createrawtx_reference`
结果：此时创建新资产会在 第一个交易输入的地址上(即：在第一步引入)。
6. 对创建的交易进行签名：`omnicashd-cli signrawtransaction`
7. 发送交易：`omnicashd-cli sendrawtransaction`

### 创建众筹资产
1. 添加交易输入：   `omnicashd-cli omni_createrawtx_input `
2. 生成创建众筹资产的载荷数据： `omnicashd-cli omni_createpayload_issuancecrowdsale`
3. 创建交易输出，将创建的omni载荷数据添加进交易输出：   `omnicashd-cli omni_createrawtx_opreturn`
4. 创建输出，进行找零：`omnicashd-cli omni_createrawtx_reference`
结果：此时创建新资产会在 第一个交易输入的地址上(即：在第一步引入)。
6. 对创建的交易进行签名：`omnicashd-cli signrawtransaction`
7. 发送交易：`omnicashd-cli sendrawtransaction`

### 关闭众筹资产
1. 添加交易输入：   `omnicashd-cli omni_createrawtx_input `
2. 生成创建众筹资产的载荷数据： `omnicashd-cli omni_createpayload_closecrowdsale`
3. 创建交易输出，将创建的omni载荷数据添加进交易输出：   `omnicashd-cli omni_createrawtx_opreturn`
4. 创建输出，进行找零：`omnicashd-cli omni_createrawtx_reference`
6. 对创建的交易进行签名：`omnicashd-cli signrawtransaction`
7. 发送交易：`omnicashd-cli sendrawtransaction`

### 创建可管理资产
1. 添加交易输入：   `omnicashd-cli omni_createrawtx_input `
2. 生成创建可管理资产的载荷数据： `omnicashd-cli omni_createpayload_issuancemanaged`
3. 创建交易输出，将创建的omni载荷数据添加进交易输出：   `omnicashd-cli omni_createrawtx_opreturn`
4. 创建输出，进行找零：`omnicashd-cli omni_createrawtx_reference`
结果：此时创建新资产会在 第一个交易输入的地址上(即：在第一步引入)。
6. 对创建的交易进行签名：`omnicashd-cli signrawtransaction`
7. 发送交易：`omnicashd-cli sendrawtransaction`

### 增发资产
1. 添加交易输入：   `omnicashd-cli omni_createrawtx_input `
2. 生成创建可管理资产的载荷数据： `omnicashd-cli omni_createpayload_grant`
3. 创建交易输出，将创建的omni载荷数据添加进交易输出：   `omnicashd-cli omni_createrawtx_opreturn`
4. 创建输出，进行找零：`omnicashd-cli omni_createrawtx_reference`
结果：此时创建新资产会在 第一个交易输入的地址上(即：在第一步引入)。
6. 对创建的交易进行签名：`omnicashd-cli signrawtransaction`
7. 发送交易：`omnicashd-cli sendrawtransaction`

### 销毁资产
1. 添加交易输入：   `omnicashd-cli omni_createrawtx_input `
2. 生成创建可管理资产的载荷数据： `omnicashd-cli omni_createpayload_revoke `
3. 创建交易输出，将创建的omni载荷数据添加进交易输出：   `omnicashd-cli omni_createrawtx_opreturn`
4. 创建输出，进行找零：`omnicashd-cli omni_createrawtx_reference`
结果：此时创建新资产会在 第一个交易输入的地址上(即：在第一步引入)。
6. 对创建的交易进行签名：`omnicashd-cli signrawtransaction`
7. 发送交易：`omnicashd-cli sendrawtransaction`

### 进行空投
1. 添加交易输入：   `omnicashd-cli omni_createrawtx_input `(注意：第一个输入必须含有足够的空投资产)
2. 生成创建可管理资产的载荷数据： `omnicashd-cli omni_createpayload_sto`
3. 创建交易输出，将创建的omni载荷数据添加进交易输出：   `omnicashd-cli omni_createrawtx_opreturn`
4. 创建输出，进行找零：`omnicashd-cli omni_createrawtx_reference`
结果：此时创建新资产会在 第一个交易输入的地址上(即：在第一步引入)。
6. 对创建的交易进行签名：`omnicashd-cli signrawtransaction`
7. 发送交易：`omnicashd-cli sendrawtransaction`

### 更改资产发行者
1. 添加交易输入：   `omnicashd-cli omni_createrawtx_input `(注意：第一个输入必须为资产的发行地址)
2. 生成创建可管理资产的载荷数据： `omnicashd-cli omni_createpayload_changeissuer`
3. 创建交易输出，将创建的omni载荷数据添加进交易输出：   `omnicashd-cli omni_createrawtx_opreturn`
4. 创建输出，进行找零：`omnicashd-cli omni_createrawtx_reference`
5. 创建资产接收者输出：omnicashd-cli omni_createrawtx_reference
6. 对创建的交易进行签名：`omnicashd-cli signrawtransaction`
7. 发送交易：`omnicashd-cli sendrawtransaction`

## 查询使用的RPC
`omnicashd-cli getrawtransaction "txid"`: 获取指定交易哈希的16进制交易数据
`omnicashd-cli decoderawtransaction "rawtx"`: 对获取到的16进制数据进行解码
`omnicashd-cli signrawtransaction "rawtx"`: 对原始交易进行签名
`omnicashd-cli sendrawtransaction "rawtx"`: 发送签名后的交易
`omnicashd-cli listunspent (成熟度0, 1 ...)`: 列出当前钱包中可以使用的所有资金
`omnicashd-cli omni_gettransaction "txid"`: 获取omni交易的解析
`omnicashd-cli omni_getbalance  "address" propertyID `: 获取指定地址指定资产的余额
`omnicashd-cli  getproperty_MP propertyID`: 列写omni系统中指定资产的基础信息
`omnicashd-cli listblocktransactions_MP height`: 列出某个块高度含有的所有omni交易
`omnicashd-cli omni_listpendingtransactions`: 列出当前节点的交易池中所有未确认的omni交易
`omnicashd-cli omni_getpayload "txid"`: 返回指定omni交易的载荷数据
`omnicashd-cli getsto_MP "txid" "*"`: 列出指定空投交易的所有参与者，以及金额信息
`omnicashd-cli getgrants_MP propertyID`: 返回指定的管理资产的增发/销毁信息

## RPC参数，返回值详解

### whc_getactivecrowd
解释：获取指定地址的活跃众筹；
调用：`wormholed-cli whc_getactivecrowd address`
参数：`address` : 指定查询地址
返回值
    
    *   该地址存在活跃众筹时，返回活跃众筹资产的信息
    *   不存在活跃众筹，返回`{}`.
示例如下
>wormholed-cli whc_getactivecrowd  qq893ghdg697e5t5anh5fqpxwxxhw3akyu9l7wej0q
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

返回值字段描述

*   propertyid : 资产ID
*   name : 资产名称
*   category : 资产类别
*   subcategory : 资产子类别
*   data : 资产信息
*   url : 资产的URL
*   precision : 资产精度
*   issuer : 发行者
*   creationtxid : 创建该资产的交易ID
*   totaltokens : 总共发行的众筹资产数量


### whc_getallbalancesforaddress
解释：获取指定地址所有类型token的非0余额信息；对于金额为0的token，不显示
调用：`wormholed-cli whc_getallbalancesforaddress address`
参数：`address` : 指定查询地址
返回值
    
    *   存在金额非0的token，返回该token的金额信息；否则返回`{}`.

示例如下
>   wormholed-cli whc_getallbalancesforaddress qr3pzyxl33vdhga54rvh80s62pjznl9eyu9k7q9tmv
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
      },
      {
        "propertyid": 5,
        "balance": "10000.0",
        "reserved": "0.0"
      }
]

返回值字段描述

*   propertyid : 资产ID
*   balance : 可用余额
*   reserved : 冻结余额，当前版本不可用


### whc_getallbalancesforid
解释：获取当前块链中含有指定资产的所有地址及该资产的金额信息
调用：`wormholed-cli whc_getallbalancesforid property`
参数：`property` : 资产ID
返回值

*   含有该资产的地址列表

示例如下
>   wormholed-cli whc_getallbalancesforid 1
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
  },
  {
    "address": "bitcoincash:qrnd0nwujrxt23rz2w9nxxjd2lf6s4dcvug27k6n5g",
    "balance": "1.00000000",
    "reserved": "0.00000000"
  }
]

返回值字段描述

*   address : 地址
*   balance : 可用余额
*   reserved : 冻结余额，当前版本不可用


###  whc_getbalanceshash
描述：返回当前节点最高区块状态下，指定资产在系统中存在的状态哈希
调用：`wormholed-cli whc_getbalanceshash 1`
参数：指定资产ID
返回值：资产在系统中存在的状态哈希
示例如下
>   wormholed-cli whc_getbalanceshash 1
{
  "block": 542347,
  "blockhash": "000000000000000000f6d63c8adeff1f4bc6fe3618d73f74813c05f08761060a",
  "propertyid": 1,
  "balanceshash": "f4a051549368b79409b25ad5c3dba4a9e8b0434996d88be8281969f204b35dee"
}

返回值字段描述

*   block : 当前节点的最高区块号
*   blockhash : 该区块的哈希
*   propertyid : 资产ID
*   balanceshash : 资产在系统中存在的状态哈希


### whc_getbalance
描述：返回指定地址，指定资产的金额信息
调用：`wormholed-cli whc_getbalance address propertyid`
参数

*   `address` ：指定的地址
*   `propertyid` ：资产ID
返回值：指定地址的指定资产的金额信息
示例如下
>   wormholed-cli whc_getbalance qqmrktdkuj0qtu0dyef0h2xkn7u6stycuvk70k0ups 1
{
  "balance": "5.00000000",
  "reserved": "0.00000000"
}

返回值字段描述

*   balance : 可用余额
*   reserved : 冻结余额，当前版本不可用


### whc_getcrowdsale
描述：获取众筹资产的信息
调用：`wormholed-cli whc_getcrowdsale propertyid (verbose)`
参数：
    
*   propertyid ：众筹资产的ID
*   verbose ：true, 列出众筹参与的信息；false, 不列出众筹参与的信息

返回值：返回众筹资产的信息
示例如下
>    wormholed-cli  whc_getcrowdsale  168 true
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

返回值字段描述

*   propertyid ：资产ID
*   name ：资产名称
*   active ：众筹是否处于活跃状态
*   issuer ：众筹的发行者
*   propertyiddesired ：募集的资产ID
*   precision ：众筹资产的精度
*   tokensperunit ： 该众筹token的单价，即1WHC等于多少token。
*   earlybonus ：早鸟奖励
*   starttime ： 该众筹开始的时间
*   deadline ： 该众筹截止时间
*   amountraised ：已募集到的资金
*   tokensissued ： 众筹资产发行的数量
*   addedissuertokens ：当众筹关闭时，未售完的众筹数量，这些数量的token 会计入发行者的账户地址
*   participanttransactions ：众筹参与者的信息
        
    *   txid：参与众筹的交易ID
    *   amountsent：参与众筹的WHC金额
    *   participanttokens：购买到的众筹token数量

    
### whc_getcurrentconsensushash
描述：获取当前wormhole系统的状态哈希
调用：`wormholed-cli whc_getcurrentconsensushash`
返回值：系统状态的哈希
示例如下
>   wormholed-cli whc_getcurrentconsensushash
{
  "block": 542354,
  "blockhash": "00000000000000000104a3002e3ddf7ccc835751e09ef3335a22078b129c2c71",
  "consensushash": "95e4e7fa0d92f84b18ec45ad9b39fdafcc5200ba8175227d7d571864aa5948c9"
}

返回值字段描述

*   block : 当前节点的最高区块号
*   blockhash : 该区块的哈希
*   consensushash : 系统状态的哈希

### whc_getgrants
描述：返回管理资产的增发或销毁信息
调用：`wormholed-cli whc_getgrants propertyid`
参数：资产ID
返回值：管理资产的增发或销毁信息
示例如下
>   wormholed-cli whc_getgrants 166
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

返回值字段描述

*   propertyid ： 资产ID
*   name ：资产名称
*   issuer ：资产发行者
*   creationtxid ：创建该笔资产的交易ID
*   totaltokens ： 当前token的发行量
*   issuances ：资产管理的信息
    * txid ：增发或销毁的交易ID
    * grant：增发的token数量
    * revoke：销毁的token数量


### whc_getpayload
描述：获取wormhole的交易的协议数据
调用：`wormholed-cli whc_getpayload txid`
参数：交易ID
返回值：指定交易中wormhole协议的数据
示例如下
>   wormholed-cli whc_getpayload a82b29d69538268fb67df384b9be7128456724e6fa69e4eb387944cb78ed9d66
{
  "payload": "00000036010005000000006263657874207265706f7369746f7279006763617368206361736877616c6c657420636173687574696c206e65757472696e6f00626c6f636b636861696e20657874656e73696f6e20646576656c6f706d656e740068747470733a2f2f6769746875622e636f6d2f626365787400666f72207468652066757475726500",
  "payloadsize": 136
}

返回值字段描述

*   payload ：载荷数据
*   payloadsize ：载荷数据的长度


### whc_getproperty
描述：获取指定资产信息
调用：`wormholed-cli whc_getproperty propertyid`
参数：资产ID
返回值：该资产的详细信息
示例如下
>   wormholed-cli whc_getproperty 168
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


返回值字段描述

*   propertyid ：资产ID
*   name ：资产名称
*   category : 资产类别
*   subcategory : 资产子类别
*   data : 资产信息
*   url : 资产的URL
*   precision : 资产精度
*   issuer : 发行者
*   fixedissuance : 是否属于固定资产
*   managedissuance : 是否属于可管理资产
*   creationtxid : 创建该资产的交易ID
*   totaltokens : 总共发行的众筹资产数量


### whc_getsto
描述：获取空投相关的信息
调用：`wormholed-cli whc_getsto txid recipientfilter`
参数

*   txid : 空投的交易ID
*   recipientfilter : 过滤器；指定地址时，只显示该接收的空投金额信息；当为`*`时，默认显示所有账户接收的空投金额信息。
返回值：空投的详细信息

示例如下
>   wormholed-cli whc_getsto 403ec9b6f8b142485ea514d52bc4c782f008021a261f637028a28e1a64681d1b
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


返回值字段描述

*   txid : 该空投交易ID
*   fee  : 该交易的BCH手续费
*   sendingaddress : 空投交易的发送者
*   ismine ：该发送地址是否属于节点钱包中的地址
*   version ：空投类型的版本号
*   type_int ：空投交易类型
*   type ： 空投交易类型
*   propertyid ：空投的资产ID
*   precision ：空投的资产精度
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
>   wormholed-cli whc_gettransaction a82b29d69538268fb67df384b9be7128456724e6fa69e4eb387944cb78ed9d66
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

返回值字段描述

*   txid : 交易ID
*   fee  : 该交易的BCH手续费
*   sendingaddress : 交易的发送者
*   ismine ：该发送地址是否属于节点钱包中的地址
*   version ：wormhole类型的版本号
*   type_int ：wormhole的交易类型
*   type ： wormhole的交易类型
*   propertyid ：如为创建资产类型的交易，标识资产ID
*   precision ：资产精度
*   ecosystem ：该资产所在的生态体系
*   category ： 创建的资产类别
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
>   wormholed-cli whc_listblocktransactions 541634
[
  "599e5126759a98c977fce4056b14628f4e175d757d090ed11809c9f0474e6d55",
  "fa072ca373c6a38248207eab6a4e85933792628188f6ba6eb99a0fa719d8e808",
  "ec451d67689ed2d990652a013b1af87edce694f5675c11b5063c89889f5fa8ed",
  "548a10ca6c0b36bed39a78e4d47636fa16ccd3b9ba0dadb5882deb4933f83336",
  "72d266f60e6c7ddb64b5009b59bc263da4ee1be89257b1fd1625a3c674b23795",
  "a82b29d69538268fb67df384b9be7128456724e6fa69e4eb387944cb78ed9d66",
  "0956f48b0b097df6d3aa2d34acfe75a362a185fdfe32f90b5683f178558d5569"
]

### whc_listpendingtransactions
描述：查询节点交易池中相关地址未确认的wormhole交易
调用：`wormholed-cli whc_listpendingtransactions address`
参数：过滤地址
返回值：指定地址未确认的交易信息列表
示例如下
>   wormholed-cli whc_listpendingtransactions qpadl79yr4hh0fym4gw73mxp3rm4325knqe7fj6a9s
[
]

### whc_listproperties
描述：列出整个系统的所有资产信息
调用：`wormholed-cli whc_listproperties`
返回值：系统的所有资产列表
示例如下
>   wormholed-cli whc_listproperties
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
>wormholed-cli  whc_listtransactions qz04wg2jj75x34tge2v8w0l6r0repfcvcygv3t7sg5
[
]

