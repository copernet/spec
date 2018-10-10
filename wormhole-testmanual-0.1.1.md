测试手册WHC version 0.1.1 

一、测试环境搭建

1. 软件下载：https://github.com/copernet/wormhole/releases/tag/Earth-0.1.1-pre-release

2. 编译安装

   Unix平台：https://github.com/copernet/wormhole/blob/master/doc/build-unix.md

   OSX平台：https://github.com/copernet/wormhole/blob/master/doc/build-osx.md

   Windows平台：https://github.com/copernet/wormhole/blob/master/doc/build-windows.md

3. 运行及数据同步

   初次运行0.1.1版本的代码，使用如下命令：`wormholed -startclean=1 -daemon`

   当0.1.1版本启动，且数据同步完成后，下次软件重启时，使用如下命令：`wormholed -daemon`

二、基础环境准备

基础环境准备包括地址生成，WHC的获取。

详见https://github.com/copernet/spec/blob/master/wormhole-testmanual-0.0.6.md

三、资产冻结功能

1、创建开启冻结功能的可管理资产

```
root@iZhp3e7c2bja4qiyiy3gakZ:~/wormhole/src# wormholed-cli whc_sendissuancemanaged qpjua0mvqpnyxddavqys2j3d8wuewarmnvx3kqha2q 1 8 1 "test freeze" "frozentoken" "www" "hello" ""
e32bff59d6088f2d6088ae90b31807cf4e50fde2f20bcdd3f1ca7b904b0fe2b6
```

2、查看交易信息（确认后）

```
root@iZhp3e7c2bja4qiyiy3gakZ:~/wormhole/src# wormholed-cli whc_gettransaction e32bff59d6088f2d6088ae90b31807cf4e50fde2f20bcdd3f1ca7b904b0fe2b6
{
  "txid": "e32bff59d6088f2d6088ae90b31807cf4e50fde2f20bcdd3f1ca7b904b0fe2b6",
  "fee": "306",
  "sendingaddress": "bchtest:qpjua0mvqpnyxddavqys2j3d8wuewarmnvx3kqha2q",
  "ismine": true,
  "version": 0,
  "type_int": 54,
  "type": "Create Property - Manual",
  "propertyid": 320,
  "precision": "8",
  "ecosystem": "main",
  "category": "test freeze",
  "subcategory": "frozentoken",
  "propertyname": "www",
  "data": "",
  "url": "hello",
  "amount": "0.00000000",
  "valid": true,
  "blockhash": "00000000001990126e2b92cdc578bc3ba3152e1f0095b327e651dbd3ea8bd102",
  "blocktime": 1538992160,
  "positioninblock": 1,
  "block": 1260812,
  "confirmations": 24
}
```

3、查看资产信息

```
root@iZhp3e7c2bja4qiyiy3gakZ:~/wormhole/src# wormholed-cli whc_getproperty 320
{
  "propertyid": 320,
  "name": "www",
  "category": "test freeze",
  "subcategory": "frozentoken",
  "data": "",
  "url": "hello",
  "precision": 8,
  "issuer": "bchtest:qpjua0mvqpnyxddavqys2j3d8wuewarmnvx3kqha2q",
  "creationtxid": "e32bff59d6088f2d6088ae90b31807cf4e50fde2f20bcdd3f1ca7b904b0fe2b6",
  "fixedissuance": false,
  "managedissuance": true,
  "freezingenabled": true,
  "totaltokens": "0.00000000"
}

```

4、增发资产

```
root@iZhp3e7c2bja4qiyiy3gakZ:~/wormhole/src# wormholed-cli whc_sendgrant qpjua0mvqpnyxddavqys2j3d8wuewarmnvx3kqha2q "" 320 "1000" 
906db007e1c4dbb81362c21b6433183538110a7aa0812679c7dae0115e89c1e6
```

5、查看资产信息

```
root@iZhp3e7c2bja4qiyiy3gakZ:~/wormhole/src# wormholed-cli whc_getproperty 320
{
  "propertyid": 320,
  "name": "www",
  "category": "test freeze",
  "subcategory": "frozentoken",
  "data": "",
  "url": "hello",
  "precision": 8,
  "issuer": "bchtest:qpjua0mvqpnyxddavqys2j3d8wuewarmnvx3kqha2q",
  "creationtxid": "e32bff59d6088f2d6088ae90b31807cf4e50fde2f20bcdd3f1ca7b904b0fe2b6",
  "fixedissuance": false,
  "managedissuance": true,
  "freezingenabled": true,
  "totaltokens": "1000.00000000"
}
```

6、转移资产

```
root@iZhp3e7c2bja4qiyiy3gakZ:~/wormhole/src# wormholed-cli whc_send qpjua0mvqpnyxddavqys2j3d8wuewarmnvx3kqha2q qqjm9ellvuf2s989ljjlr7qq39xhtm9pjs8tvvz7xf 320 200
5198337cec17593969d7a430f9bda4d003bdd8e46ea22d16a801a24c806099f7
```

7、显示余额

```
root@iZhp3e7c2bja4qiyiy3gakZ:~/wormhole/src# wormholed-cli whc_getallbalancesforid 320
[
  {
    "address": "bchtest:qpjua0mvqpnyxddavqys2j3d8wuewarmnvx3kqha2q",
    "balance": "2800.00000000",
    "reserved": "0.00000000"
  },
  {
    "address": "bchtest:qqpj0yu8w9ukg7x4h83xx7a4nj8f7mssh5dgn6flfu",
    "balance": "2200.00000000",
    "reserved": "0.00000000"
  }
]
```

8、冻结资产

```
root@iZhp3e7c2bja4qiyiy3gakZ:~/wormhole/src# ./wormholed-cli whc_sendfreeze qpjua0mvqpnyxddavqys2j3d8wuewarmnvx3kqha2q 320 "100.0" qqpj0yu8w9ukg7x4h83xx7a4nj8f7mssh5dgn6flfu
45a9e5702c7d5c9836d77a5f571059a2b50fe32e39bf7bf0c0d7951392cb4e0b
```

9、查看交易信息

```
root@iZhp3e7c2bja4qiyiy3gakZ:~/wormhole/src# wormholed-cli whc_gettransaction 45a9e5702c7d5c9836d77a5f571059a2b50fe32e39bf7bf0c0d7951392cb4e0b
{
  "txid": "45a9e5702c7d5c9836d77a5f571059a2b50fe32e39bf7bf0c0d7951392cb4e0b",
  "fee": "333",
  "sendingaddress": "bchtest:qpjua0mvqpnyxddavqys2j3d8wuewarmnvx3kqha2q",
  "referenceaddress": "bchtest:qqpj0yu8w9ukg7x4h83xx7a4nj8f7mssh5dgn6flfu",
  "ismine": true,
  "version": 0,
  "type_int": 185,
  "type": "Freeze Property Tokens",
  "valid": true,
  "blockhash": "00000000001809e1d4e7fde4965a2af2fd239ca50923ffbe2554551ef6d0d1a4",
  "blocktime": 1538999325,
  "positioninblock": 2,
  "block": 1261000,
  "confirmations": 2
}
```

10、转移冻结资产

```
root@iZhp3e7c2bja4qiyiy3gakZ:~/wormhole/src# ./wormholed-cli whc_send qqpj0yu8w9ukg7x4h83xx7a4nj8f7mssh5dgn6flfu qpjua0mvqpnyxddavqys2j3d8wuewarmnvx3kqha2q 320 100
5652485cbd146c7dfcc0e3a26698992fb45b550f313ea9d0cb41fa219f47134f
```

11、查看交易确认信息

```
root@iZhp3e7c2bja4qiyiy3gakZ:~/wormhole/src# wormholed-cli whc_gettransaction 5652485cbd146c7dfcc0e3a26698992fb45b550f313ea9d0cb41fa219f47134f
{
  "txid": "5652485cbd146c7dfcc0e3a26698992fb45b550f313ea9d0cb41fa219f47134f",
  "fee": "311",
  "sendingaddress": "bchtest:qqpj0yu8w9ukg7x4h83xx7a4nj8f7mssh5dgn6flfu",
  "referenceaddress": "bchtest:qpjua0mvqpnyxddavqys2j3d8wuewarmnvx3kqha2q",
  "ismine": true,
  "version": 0,
  "type_int": 0,
  "type": "Simple Send",
  "propertyid": 320,
  "precision": "8",
  "amount": "100.00000000",
  "valid": false,
  "invalidreason": "Sender is frozen for the property",
  "blockhash": "00000000d1b57e14152b78b71933ed7313ab4524fa18f1265609aaf2797ff226",
  "blocktime": 1539005604,
  "positioninblock": 1,
  "block": 1261007,
  "confirmations": 1
}
```

此处显示交易无效，原因：Sender is frozen for the property

四、资产解冻功能

1、解冻资产

```
./wormholed-cli whc_sendunfreeze qpjua0mvqpnyxddavqys2j3d8wuewarmnvx3kqha2q 320 "100.0" qqpj0yu8w9ukg7x4h83xx7a4nj8f7mssh5dgn6flfu
4d7e239fbc1a71ce7b27ae7b6bc4c557131973505f0d1701377d0302177390f9
```

2、查看交易信息

```
root@iZhp3e7c2bja4qiyiy3gakZ:~/wormhole/src# ./wormholed-cli whc_gettransaction 8217763f8ea9958ac08d6cbf203312174a9273534d6d9ddc1e7a70c2fce75fab
{
  "txid": "8217763f8ea9958ac08d6cbf203312174a9273534d6d9ddc1e7a70c2fce75fab",
  "fee": "332",
  "sendingaddress": "bchtest:qpjua0mvqpnyxddavqys2j3d8wuewarmnvx3kqha2q",
  "referenceaddress": "bchtest:qqpj0yu8w9ukg7x4h83xx7a4nj8f7mssh5dgn6flfu",
  "ismine": true,
  "version": 0,
  "type_int": 186,
  "type": "Unfreeze Property Tokens",
  "valid": true,
  "blockhash": "000000000025e400ad763a3a4c4969a8ee5476a85e51e1ff32297b09a04f6db1",
  "blocktime": 1539001935,
  "positioninblock": 4,
  "block": 1261003,
  "confirmations": 116
}
```

3、转移解冻资产

```
./wormholed-cli whc_send qqpj0yu8w9ukg7x4h83xx7a4nj8f7mssh5dgn6flfu qpjua0mvqpnyxddavqys2j3d8wuewarmnvx3kqha2q 320 300
813c767626c12f7af94095f67ad94c58f64394f9800fc7e7940d6fef84b2a791
```

4、查看交易信息

```
root@iZhp3e7c2bja4qiyiy3gakZ:~/wormhole/src# ./wormholed-cli whc_gettransaction 813c767626c12f7af94095f67ad94c58f64394f9800fc7e7940d6fef84b2a791
{
  "txid": "813c767626c12f7af94095f67ad94c58f64394f9800fc7e7940d6fef84b2a791",
  "fee": "311",
  "sendingaddress": "bchtest:qqpj0yu8w9ukg7x4h83xx7a4nj8f7mssh5dgn6flfu",
  "referenceaddress": "bchtest:qpjua0mvqpnyxddavqys2j3d8wuewarmnvx3kqha2q",
  "ismine": true,
  "version": 0,
  "type_int": 0,
  "type": "Simple Send",
  "propertyid": 320,
  "precision": "8",
  "amount": "300.00000000",
  "valid": true,
  "blockhash": "0000000052a2640baf8c1d6f4aac001c306dc5085b3b39c20594387171a4bfdc",
  "blocktime": 1539050941,
  "positioninblock": 15,
  "block": 1261119,
  "confirmations": 1
}
```

5、查看资产余额

```
root@iZhp3e7c2bja4qiyiy3gakZ:~/wormhole/src# ./wormholed-cli whc_getallbalancesforid 320
[
  {
    "address": "bchtest:qqjm9ellvuf2s989ljjlr7qq39xhtm9pjs8tvvz7xf",
    "balance": "200.00000000",
    "reserved": "0.00000000"
  },
  {
    "address": "bchtest:qpjua0mvqpnyxddavqys2j3d8wuewarmnvx3kqha2q",
    "balance": "2900.00000000",
    "reserved": "0.00000000"
  },
  {
    "address": "bchtest:qqpj0yu8w9ukg7x4h83xx7a4nj8f7mssh5dgn6flfu",
    "balance": "1900.00000000",
    "reserved": "0.00000000"
  }
]
```

6、冻结资产

```
root@iZhp3e7c2bja4qiyiy3gakZ:~/wormhole/src# ./wormholed-cli whc_sendfreeze qpjua0mvqpnyxddavqys2j3d8wuewarmnvx3kqha2q 320 "100" qqpj0yu8w9ukg7x4h83xx7a4nj8f7mssh5dgn6flfu
6da4d826d2ab944e8ceb78759850e8ee763ee2916c2c2a2bf90690a695c74ad9
```

7、查看冻结资产

```
root@iZhp3e7c2bja4qiyiy3gakZ:~/wormhole/src# ./wormholed-cli whc_getfrozenbalance qqpj0yu8w9ukg7x4h83xx7a4nj8f7mssh5dgn6flfu 320
{
  "frozen": true,
  "balance": "1900.00000000"
}
```

```
root@iZhp3e7c2bja4qiyiy3gakZ:~/wormhole/src# ./wormholed-cli whc_getfrozenbalanceforid 320
[
  {
    "address": "bchtest:qqpj0yu8w9ukg7x4h83xx7a4nj8f7mssh5dgn6flfu",
    "balance": "1900.00000000"
  }
]
```

```
root@iZhp3e7c2bja4qiyiy3gakZ:~/wormhole/src# ./wormholed-cli whc_getfrozenbalanceforaddress qqpj0yu8w9ukg7x4h83xx7a4nj8f7mssh5dgn6flfu
[
  {
    "propertyid": 320,
    "balance": "1900.00000000"
  }
]
```