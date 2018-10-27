测试手册WHC version 0.1.2

一、测试环境搭建

1. 软件下载：https://github.com/copernet/wormhole/releases/tag/Earth-0.1.2-pre-release

2. 编译安装

   Unix平台：https://github.com/copernet/wormhole/blob/master/doc/build-unix.md

   OSX平台：https://github.com/copernet/wormhole/blob/master/doc/build-osx.md

   Windows平台：https://github.com/copernet/wormhole/blob/master/doc/build-windows.md

3. 运行及数据同步

   初次运行0.1.2版本的代码，使用如下命令：`wormholed -startclean=1 -daemon`

   当0.1.2版本启动，且数据同步完成后，下次软件重启时，使用如下命令：`wormholed -daemon`

二、基础环境准备

基础环境准备包括地址生成，WHC的获取。

详见https://github.com/copernet/spec/blob/master/wormhole-testmanual-0.0.6.md

三、资产冻结功能

1、创建开启冻结功能的可管理资产

```
wormholed-cli whc_sendissuancemanaged qpfgew6hes5k9e40mzcngz86hrvc786f8ycu9snrva 1 8 1 "freeze-coin" "frozentoken alfa" "www" "hello" ""
643acc7e2bf17d16e4b9ad5e4c04a8491b2ceaec0aea3d2b3524c8fdab24f7f3
```

2、查看交易信息

```
wormholed-cli whc_gettransaction 643acc7e2bf17d16e4b9ad5e4c04a8491b2ceaec0aea3d2b3524c8fdab24f7f3
{
  "txid": "643acc7e2bf17d16e4b9ad5e4c04a8491b2ceaec0aea3d2b3524c8fdab24f7f3",
  "fee": "281",
  "sendingaddress": "bchtest:qpfgew6hes5k9e40mzcngz86hrvc786f8ycu9snrva",
  "ismine": true,
  "version": 0,
  "type_int": 54,
  "type": "Create Property - Manual",
  "propertyid": 369,
  "precision": "8",
  "ecosystem": "main",
  "category": "freeze-coin",
  "subcategory": "frozentoken alfa",
  "propertyname": "www",
  "data": "",
  "url": "hello",
  "amount": "0.00000000",
  "valid": true,
  "blockhash": "00000000000a8cbe9e0c1d3f36a8a68f80889618f5c834552b7db8c096680aa1",
  "blocktime": 1539347623,
  "positioninblock": 35,
  "block": 1262182,
  "confirmations": 1
}
```

3、增发资产

```
wormholed-cli whc_sendgrant qpfgew6hes5k9e40mzcngz86hrvc786f8ycu9snrva "" 369 "10000"
0d0f381bbfbbbe1642fc381c99912ee63d4c63eb54d410b6c0117080c8a9104d
```

4、转账给待冻结地址

```
wormholed-cli whc_send qpfgew6hes5k9e40mzcngz86hrvc786f8ycu9snrva qzhwj99z3dh6a9fa5349qrx2v964gpvqvutmqaastk 369 "1000"
3a1f14e9f7a3094e91e9770266827dacd4292cc2e87b639b8f0a4114d622f15b
```

5、冻结资产

```
wormholed-cli whc_sendfreeze qpfgew6hes5k9e40mzcngz86hrvc786f8ycu9snrva 369 "100" qzhwj99z3dh6a9fa5349qrx2v964gpvqvutmqaastk67ea77b4223e02cec1eb2e98c043b16945050b119417db8cb73a48c0e9b1cfc4
```

6、查看交易信息

```
wormholed-cli whc_gettransaction 67ea77b4223e02cec1eb2e98c043b16945050b119417db8cb73a48c0e9b1cfc4
{
  "txid": "67ea77b4223e02cec1eb2e98c043b16945050b119417db8cb73a48c0e9b1cfc4",
  "fee": "299",
  "sendingaddress": "bchtest:qpfgew6hes5k9e40mzcngz86hrvc786f8ycu9snrva",
  "referenceaddress": "bchtest:qzhwj99z3dh6a9fa5349qrx2v964gpvqvutmqaastk",
  "ismine": true,
  "version": 0,
  "type_int": 185,
  "type": "Freeze Property Tokens",
  "valid": true,
  "blockhash": "0000000000a01aca27be516677d1e644171107e22f2ed518ac940317babd4eb8",
  "blocktime": 1539351245,
  "positioninblock": 13,
  "block": 1262185,
  "confirmations": 160
}
```

7、转移冻结资产

```
wormholed-cli whc_send qzhwj99z3dh6a9fa5349qrx2v964gpvqvutmqaastk qpfgew6hes5k9e40mzcngz86hrvc786f8ycu9snrva 369 100
ce546e1c759999a13944641724aed3adcf8d3dd4c0727f450ca3e2e76ebea0cf
```

8、交易报错：Sender is frozen for the property

```
wormholed-cli whc_gettransaction ce546e1c759999a13944641724aed3adcf8d3dd4c0727f450ca3e2e76ebea0cf
{
  "txid": "ce546e1c759999a13944641724aed3adcf8d3dd4c0727f450ca3e2e76ebea0cf",
  "fee": "281",
  "sendingaddress": "bchtest:qzhwj99z3dh6a9fa5349qrx2v964gpvqvutmqaastk",
  "referenceaddress": "bchtest:qpfgew6hes5k9e40mzcngz86hrvc786f8ycu9snrva",
  "ismine": true,
  "version": 0,
  "type_int": 0,
  "type": "Simple Send",
  "propertyid": 369,
  "precision": "8",
  "amount": "100.00000000",
  "valid": false,
  "invalidreason": "Sender is frozen for the property",
  "blockhash": "00000000000000e0c6926477c714ce17dd9d412d0011a9e724f396ea2a836627",
  "blocktime": 1539396228,
  "positioninblock": 1,
  "block": 1262346,
  "confirmations": 1
}
```

9、重复冻结

```
wormholed-cli whc_sendfreeze qpfgew6hes5k9e40mzcngz86hrvc786f8ycu9snrva 369 "100" qzhwj99z3dh6a9fa5349qrx2v964gpvqvutmqaastk
21e8993e55b37eef77ba1e3fdb96fa675d07cfa119b7471f3d2a4ef27fc5902e
```

10、交易信息

```
wormholed-cli whc_gettransaction 21e8993e55b37eef77ba1e3fdb96fa675d07cfa119b7471f3d2a4ef27fc5902e
{
  "txid": "21e8993e55b37eef77ba1e3fdb96fa675d07cfa119b7471f3d2a4ef27fc5902e",
  "fee": "299",
  "sendingaddress": "bchtest:qpfgew6hes5k9e40mzcngz86hrvc786f8ycu9snrva",
  "referenceaddress": "bchtest:qzhwj99z3dh6a9fa5349qrx2v964gpvqvutmqaastk",
  "ismine": true,
  "version": 0,
  "type_int": 185,
  "type": "Freeze Property Tokens",
  "valid": true,
  "blockhash": "000000000000010ec3ba80ecc61aae6fe8d7abd09e07f567bf12ff4bdc4dc939",
  "blocktime": 1539396846,
  "positioninblock": 8,
  "block": 1262347,
  "confirmations": 1
}
```

11、转移管理权给未冻结地址 

```
wormholed-cli whc_sendchangeissuer qpfgew6hes5k9e40mzcngz86hrvc786f8ycu9snrva qpwyl4vh9np38qeqnu9t5zsn057yvvxv8cf3a7ezvg 369
27c09f37aaa31478f9bebd6b43d39bf2f99f0faaf7b56e3480f911fedfa2756a
```

12、原始发行者冻结资产

```
wormholed-cli whc_sendfreeze qpfgew6hes5k9e40mzcngz86hrvc786f8ycu9snrva 369 "100" qzhwj99z3dh6a9fa5349qrx2v964gpvqvutmqaastk
error code: -3
error message:
Sender is not authorized to manage the property 
```

13、新管理者冻结资产

```
wormholed-cli whc_sendfreeze qpwyl4vh9np38qeqnu9t5zsn057yvvxv8cf3a7ezvg 369 "100" qzhwj99z3dh6a9fa5349qrx2v964gpvqvutmqaastk
635638676f541d6323efbba55277aeb461c032c352bc4c48dba8bb7d0b053c8c
```

14、查看交易信息

```
wormholed-cli whc_gettransaction 635638676f541d6323efbba55277aeb461c032c352bc4c48dba8bb7d0b053c8c
{
  "txid": "635638676f541d6323efbba55277aeb461c032c352bc4c48dba8bb7d0b053c8c",
  "fee": "299",
  "sendingaddress": "bchtest:qpwyl4vh9np38qeqnu9t5zsn057yvvxv8cf3a7ezvg",
  "referenceaddress": "bchtest:qzhwj99z3dh6a9fa5349qrx2v964gpvqvutmqaastk",
  "ismine": true,
  "version": 0,
  "type_int": 185,
  "type": "Freeze Property Tokens",
  "valid": true,
  "blockhash": "00000000000b4de27b81abc6596989c556e45d453b1067f3fce17c54ebc8bc26",
  "blocktime": 1539398049,
  "positioninblock": 13,
  "block": 1262348,
  "confirmations": 2
}
```

15、解冻交易

```
wormholed-cli whc_sendunfreeze qpwyl4vh9np38qeqnu9t5zsn057yvvxv8cf3a7ezvg 369 "100" qzhwj99z3dh6a9fa5349qrx2v964gpvqvutmqaastk
bd1c3d58ab984b5d5689ace60821d3ad40ac82546f40e52120adc1703236e34f
```

16、交易信息

```
wormholed-cli whc_gettransaction bd1c3d58ab984b5d5689ace60821d3ad40ac82546f40e52120adc1703236e34f
{
  "txid": "bd1c3d58ab984b5d5689ace60821d3ad40ac82546f40e52120adc1703236e34f",
  "fee": "299",
  "sendingaddress": "bchtest:qpwyl4vh9np38qeqnu9t5zsn057yvvxv8cf3a7ezvg",
  "referenceaddress": "bchtest:qzhwj99z3dh6a9fa5349qrx2v964gpvqvutmqaastk",
  "ismine": true,
  "version": 0,
  "type_int": 186,
  "type": "Unfreeze Property Tokens",
  "valid": true,
  "blockhash": "00000000000000b31b79a8cdc9de73bf654a83af70bf0f23ec250541c03fcb46",
  "blocktime": 1539398926,
  "positioninblock": 5,
  "block": 1262350,
  "confirmations": 1
}
```

17、转移管理权给解冻地址

```
wormholed-cli whc_sendchangeissuer qpwyl4vh9np38qeqnu9t5zsn057yvvxv8cf3a7ezvg qzhwj99z3dh6a9fa5349qrx2v964gpvqvutmqaastk 369
3cb4379191d1dbb15e3ab873f18e3688c22785c33a31cf216afd0157aeb99dc2
```

18、查看交易信息

```
wormholed-cli whc_gettransaction 3cb4379191d1dbb15e3ab873f18e3688c22785c33a31cf216afd0157aeb99dc2
{
  "txid": "3cb4379191d1dbb15e3ab873f18e3688c22785c33a31cf216afd0157aeb99dc2",
  "fee": "272",
  "sendingaddress": "bchtest:qpwyl4vh9np38qeqnu9t5zsn057yvvxv8cf3a7ezvg",
  "referenceaddress": "bchtest:qzhwj99z3dh6a9fa5349qrx2v964gpvqvutmqaastk",
  "ismine": true,
  "version": 0,
  "type_int": 70,
  "type": "Change Issuer Address",
  "propertyid": 369,
  "precision": "8",
  "valid": true,
  "blockhash": "00000000012eed8925abd0fd4c3db3cb1c4295fb4fee272e4308775bce71abf6",
  "blocktime": 1539400191,
  "positioninblock": 8,
  "block": 1262352,
  "confirmations": 2
}
```

19、冻结交易

```
wormholed-cli whc_sendfreeze qzhwj99z3dh6a9fa5349qrx2v964gpvqvutmqaastk 369 "100" qpfgew6hes5k9e40mzcngz86hrvc786f8ycu9snrva
6be2fe99c1e98c046f9d727a6295aec0fcfe5d7635cce5e8f9b491fd9934643f
```

20、查看交易信息

```
wormholed-cli whc_gettransaction 6be2fe99c1e98c046f9d727a6295aec0fcfe5d7635cce5e8f9b491fd9934643f
{
  "txid": "6be2fe99c1e98c046f9d727a6295aec0fcfe5d7635cce5e8f9b491fd9934643f",
  "fee": "299",
  "sendingaddress": "bchtest:qzhwj99z3dh6a9fa5349qrx2v964gpvqvutmqaastk",
  "referenceaddress": "bchtest:qpfgew6hes5k9e40mzcngz86hrvc786f8ycu9snrva",
  "ismine": true,
  "version": 0,
  "type_int": 185,
  "type": "Freeze Property Tokens",
  "valid": true,
  "blockhash": "0000000000c5b9839b96cb8d30461eb56507b6a180cf69d2afb566540b811149",
  "blocktime": 1539401537,
  "positioninblock": 18,
  "block": 1262354,
  "confirmations": 9
}
```

21、转移管理权给冻结地址

```
wormholed-cli whc_sendchangeissuer qzhwj99z3dh6a9fa5349qrx2v964gpvqvutmqaastk qpfgew6hes5k9e40mzcngz86hrvc786f8ycu9snrva 369
04b4cad43ba5c19aad0a7aa438cf8d94c3b30e7b3b6967c6efbf37798b870139
```

22、查看交易信息

```
wormholed-cli whc_gettransaction 04b4cad43ba5c19aad0a7aa438cf8d94c3b30e7b3b6967c6efbf37798b870139
{
  "txid": "04b4cad43ba5c19aad0a7aa438cf8d94c3b30e7b3b6967c6efbf37798b870139",
  "fee": "272",
  "sendingaddress": "bchtest:qzhwj99z3dh6a9fa5349qrx2v964gpvqvutmqaastk",
  "referenceaddress": "bchtest:qpfgew6hes5k9e40mzcngz86hrvc786f8ycu9snrva",
  "ismine": true,
  "version": 0,
  "type_int": 70,
  "type": "Change Issuer Address",
  "propertyid": 369,
  "precision": "8",
  "valid": false,
  "invalidreason": "Attempt to change issuer to a frozen receiver",
  "blockhash": "000000007dd7bc906229e03799c0e0a51b57855e0824d62c3b0867c925d4bbbf",
  "blocktime": 1539424287,
  "positioninblock": 6,
  "block": 1262379,
  "confirmations": 1
}
```

23、查看余额

```
wormholed-cli whc_getfrozenbalance qpfgew6hes5k9e40mzcngz86hrvc786f8ycu9snrva 369
{
  "frozen": true,
  "balance": "8900.00000000"
}
```

```
wormholed-cli whc_getfrozenbalanceforid 369
[
  {
    "address": "bchtest:qpfgew6hes5k9e40mzcngz86hrvc786f8ycu9snrva",
    "balance": "8900.00000000"
  }
]
```

```
wormholed-cli whc_getfrozenbalanceforaddress qpfgew6hes5k9e40mzcngz86hrvc786f8ycu9snrva
[
  {
    "propertyid": 369,
    "balance": "8900.00000000"
  }
]
```

24、重新启动, 不带-cleanstart 参数

```
wormholed -daemon
```

25、查看余额

```
wormholed-cli whc_getfrozenbalance qpfgew6hes5k9e40mzcngz86hrvc786f8ycu9snrva 369
{
  "frozen": true,
  "balance": "8900.00000000"
}
```

