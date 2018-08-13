测试手册WHC version 0.0.6 

一、测试环境搭建
1. 软件下载：https://github.com/copernet/wormhole/releases/tag/Earth-0.0.6-pre-release
2. 编译安装

     Unix平台：https://github.com/copernet/wormhole/blob/master/doc/build-unix.md
     
     OSX平台：https://github.com/copernet/wormhole/blob/master/doc/build-osx.md
     
     Windows平台：https://github.com/copernet/wormhole/blob/master/doc/build-windows.md
3. 运行及数据同步

     初次运行0.0.6版本的代码，使用如下命令：`wormholed -startclean=1 -daemon`
     
     当0.0.6版本启动成功后，下次关机重启时，使用如下命令：`wormholed -daemon`

二、基础环境准备

1、创建钱包
    
    #u1用户创建钱包
    root@iZhp3gsd1ofz82zei2wx49Z:~/.bitcoin# wormholed-cli getnewaddress u1
    bchreg:qpzua24x08gp2czcftm87zt3yut65ahn2g549g05jz
    
2、燃烧BCH->WHC
    
    #燃烧操作
    root@iZhp3gsd1ofz82zei2wx49Z:~# wormholed-cli whc_burnbchgetwhc 10
    6b8b98f7248f96b168b8127e3eb31cabb00fd6a5eed5df6d012816d7b4335974
    
    #查询交易信息（确认后）
    root@iZhp3gsd1ofz82zei2wx49Z:~/.bitcoin# wormholed-cli whc_gettransaction 6b8b98f7248f96b168b8127e3eb31cabb00fd6a5eed5df6d012816d7b4335974
    {
      "txid": "6b8b98f7248f96b168b8127e3eb31cabb00fd6a5eed5df6d012816d7b4335974",
      "fee": "4860",
      "sendingaddress": "bchreg:qpzua24x08gp2czcftm87zt3yut65ahn2g549g05jz",
      "referenceaddress": "bchreg:pqqqqqqqqqqqqqqqqqqqqqqqqqqqqp0kvc457r8whc",
      "ismine": true,
      "version": 0,
      "type_int": 68,
      "type": "Burn BCH Get WHC",
      "propertyid": 1,
      "precision": "0",
      "mature": false,
      "amount": "1000.00000000",
      "valid": true,
      "blockhash": "0157d3d79e51c5e52be2850dbaa52f79eee457aee2de31b9118d59478d4fa419",
      "blocktime": 1533462701,
      "positioninblock": 1,
      "block": 111,
      "confirmations": 1
    }
3、查询余额

    #BCH->WHC(1:100)
    root@iZhp3gsd1ofz82zei2wx49Z:~/.bitcoin# wormholed-cli whc_getbalance qpzua24x08gp2czcftm87zt3yut65ahn2g549g05jz 1
    {
      "balance": "1000.00000000",
      "reserved": "0.00000000"
    }
    
三、众筹相关操作

1、u1用户发起众筹
    
    #发起众筹
    root@iZhp3gsd1ofz82zei2wx49Z:~/.bitcoin# wormholed-cli whc_sendissuancecrowdsale "qpzua24x08gp2czcftm87zt3yut65ahn2g549g05jz" 1  2 0 "Create Crowd Sale" "BitCoin" "Crowd_01" "http://www.baidu.com" "" 1 "100" 1535644800 10  0  20000
    2dca56a9270c3f6c907fa73b99d1fc69033346ece31e3f166b67fa489189d78f
    
    #查询交易信息（确认后）
    root@iZhp3gsd1ofz82zei2wx49Z:~/.bitcoin# wormholed-cli whc_gettransaction 2dca56a9270c3f6c907fa73b99d1fc69033346ece31e3f166b67fa489189d78f
    {
      "txid": "2dca56a9270c3f6c907fa73b99d1fc69033346ece31e3f166b67fa489189d78f",
      "fee": "6120",
      "sendingaddress": "bchreg:qpzua24x08gp2czcftm87zt3yut65ahn2g549g05jz",
      "ismine": true,
      "version": 0,
      "type_int": 51,
      "type": "Create Property - Variable",
      "propertyid": 3,
      "precision": "2",
      "ecosystem": "main",
      "category": "Create Crowd Sale",
      "subcategory": "BitCoin",
      "propertyname": "Crowd_01",
      "data": "",
      "url": "http://www.baidu.com",
      "propertyiddesired": 1,
      "tokensperunit": "100.00000000",
      "deadline": 1535644800,
      "earlybonus": 10,
      "amount": "20000.00",
      "valid": true,
      "blockhash": "67f554e9ba6f62b9cf17813bad6916208b5a402527b917f3e705f03c27fa1fd5",
      "blocktime": 1533462949,
      "positioninblock": 1,
      "block": 115,
      "confirmations": 1
    }

    #查询资产信息
    root@iZhp3gsd1ofz82zei2wx49Z:~/.bitcoin# wormholed-cli whc_getproperty 3
    {
      "propertyid": 3,
      "name": "Crowd_01",
      "category": "Create Crowd Sale",
      "subcategory": "BitCoin",
      "data": "",
      "url": "http://www.baidu.com",
      "precision": 2,
      "issuer": "bchreg:qpzua24x08gp2czcftm87zt3yut65ahn2g549g05jz",
      "creationtxid": "2dca56a9270c3f6c907fa73b99d1fc69033346ece31e3f166b67fa489189d78f",
      "fixedissuance": false,
      "managedissuance": false,
      "totaltokens": "20000.00"
    }

    #查询众筹信息
    root@iZhp3gsd1ofz82zei2wx49Z:~/.bitcoin# wormholed-cli whc_getcrowdsale 3
    {
      "propertyid": 3,
      "name": "Crowd_01",
      "active": true,
      "issuer": "bchreg:qpzua24x08gp2czcftm87zt3yut65ahn2g549g05jz",
      "propertyiddesired": 1,
      "precision": "2",
      "tokensperunit": "100.00000000",
      "earlybonus": 10,
      "starttime": 1533462949,
      "deadline": 1535644800,
      "amountraised": "0.00000000",
      "tokensissued": "20000.00",
      "addedissuertokens": "0.00"
    }

2、参与众筹

    #创建用户u2
    root@iZhp3gsd1ofz82zei2wx49Z:~/.bitcoin# wormholed-cli getnewaddress u2
    bchreg:qrkx59hgt29n980vsvkzj05ejay5en3905flhhhegl
    
    #充值bch
    充值地址：http://www.wormhole.cash/test/
    
    #充值whc（此处使用u1用户转账替代燃烧操作）
    root@iZhp3gsd1ofz82zei2wx49Z:~/.bitcoin# wormholed-cli whc_send qpzua24x08gp2czcftm87zt3yut65ahn2g549g05jz qrkx59hgt29n980vsvkzj05ejay5en3905flhhhegl 1 500
    3fe29eab43aecf7e613a0f2cf8432c31bb62654d268e0dd73d4ae1cb3029b860
    
    #u2账户有充足的bch和whc，可以参与众筹
    root@iZhp3gsd1ofz82zei2wx49Z:~/.bitcoin# wormholed-cli whc_particrowsale qrkx59hgt29n980vsvkzj05ejay5en3905flhhhegl qpzua24x08gp2czcftm87zt3yut65ahn2g549g05jz 10
    ea9698f13179494b4c705d90517e800185093a24454e86addb10f59e95af06b4
    
    #查询交易信息（确认后）
    root@iZhp3gsd1ofz82zei2wx49Z:~/.bitcoin# wormholed-cli whc_gettransaction ea9698f13179494b4c705d90517e800185093a24454e86addb10f59e95af06b4
    {
      "txid": "ea9698f13179494b4c705d90517e800185093a24454e86addb10f59e95af06b4",
      "fee": "5140",
      "sendingaddress": "bchreg:qrkx59hgt29n980vsvkzj05ejay5en3905flhhhegl",
      "referenceaddress": "bchreg:qpzua24x08gp2czcftm87zt3yut65ahn2g549g05jz",
      "ismine": true,
      "version": 0,
      "type_int": 1,
      "type": "Crowdsale Purchase",
      "propertyid": 1,
      "precision": "0",
      "amount": "10.00000000",
      "purchasedpropertyid": 3,
      "purchasedpropertyname": "Crowd_01",
      "purchasedpropertyprecision": "2",
      "purchasedtokens": "1360.64",
      "issuertokens": "0.00",
      "valid": true,
      "blockhash": "0364dc77e6d704f28559c3284fddc4aead164f6d698056ad3dec6bf4860f9db1",
      "blocktime": 1533463643,
      "positioninblock": 1,
      "block": 128,
      "confirmations": 1
    }
3、关闭众筹

    #关闭
    root@iZhp3gsd1ofz82zei2wx49Z:~/.bitcoin# wormholed-cli whc_sendclosecrowdsale qpzua24x08gp2czcftm87zt3yut65ahn2g549g05jz 3
    f40861a873a6fafadc9bc252b433268e780f232a53c4ed324ab5919d1cd23c01
    
    #查看交易信息（确认后）
    root@iZhp3gsd1ofz82zei2wx49Z:~/.bitcoin# wormholed-cli whc_gettransaction f40861a873a6fafadc9bc252b433268e780f232a53c4ed324ab5919d1cd23c01
    {
      "txid": "f40861a873a6fafadc9bc252b433268e780f232a53c4ed324ab5919d1cd23c01",
      "fee": "4300",
      "sendingaddress": "bchreg:qpzua24x08gp2czcftm87zt3yut65ahn2g549g05jz",
      "ismine": true,
      "version": 0,
      "type_int": 53,
      "type": "Close Crowdsale",
      "propertyid": 3,
      "precision": "2",
      "valid": true,
      "blockhash": "416ea77bdf23199fad2b720853c8b2fce1791f0d34002256c9d5564205c607f8",
      "blocktime": 1533463730,
      "positioninblock": 1,
      "block": 129,
      "confirmations": 1
    }
4、变更资产发行者

    #变更操作
    root@iZhp3gsd1ofz82zei2wx49Z:~/.bitcoin# wormholed-cli whc_sendchangeissuer qpzua24x08gp2czcftm87zt3yut65ahn2g549g05jz qrkx59hgt29n980vsvkzj05ejay5en3905flhhhegl 3
    84575b6fdcad0fc4b7e912c0cfec4e9a0fe43a84d916b25bc78d55835196af26
    
    #查看交易信息（确认后）
    root@iZhp3gsd1ofz82zei2wx49Z:~/.bitcoin# wormholed-cli whc_gettransaction 84575b6fdcad0fc4b7e912c0cfec4e9a0fe43a84d916b25bc78d55835196af26
    {
      "txid": "84575b6fdcad0fc4b7e912c0cfec4e9a0fe43a84d916b25bc78d55835196af26",
      "fee": "4980",
      "sendingaddress": "bchreg:qpzua24x08gp2czcftm87zt3yut65ahn2g549g05jz",
      "referenceaddress": "bchreg:qrkx59hgt29n980vsvkzj05ejay5en3905flhhhegl",
      "ismine": true,
      "version": 0,
      "type_int": 70,
      "type": "Change Issuer Address",
      "propertyid": 3,
      "precision": "2",
      "valid": true,
      "blockhash": "35401c340d51f75e03225adb95d3b66ac8a0d0966b10fbd823c735fcd45915f5",
      "blocktime": 1533463850,
      "positioninblock": 1,
      "block": 130,
      "confirmations": 1
    }
5、转账
    
    #转账前资产确认
    root@iZhp3gsd1ofz82zei2wx49Z:~/.bitcoin# wormholed-cli whc_getbalance qpzua24x08gp2czcftm87zt3yut65ahn2g549g05jz 3
    {
      "balance": "0.00",
      "reserved": "0.00"
    }
    root@iZhp3gsd1ofz82zei2wx49Z:~/.bitcoin# wormholed-cli whc_getbalance qrkx59hgt29n980vsvkzj05ejay5en3905flhhhegl 3
    {
      "balance": "1360.64",
      "reserved": "0.00"
    }
    
    #发起转账
    root@iZhp3gsd1ofz82zei2wx49Z:~/.bitcoin# wormholed-cli whc_send qrkx59hgt29n980vsvkzj05ejay5en3905flhhhegl qpzua24x08gp2czcftm87zt3yut65ahn2g549g05jz 3 100
    3e3368c0b345fb52507471ced2dcef1e2876597ac9981aa9520b75c87603cc5e
    
    #转账后确认
    root@iZhp3gsd1ofz82zei2wx49Z:~/.bitcoin# wormholed-cli whc_getbalance qrkx59hgt29n980vsvkzj05ejay5en3905flhhhegl 3
    {
      "balance": "1260.64",
      "reserved": "0.00"
    }
    root@iZhp3gsd1ofz82zei2wx49Z:~/.bitcoin# wormholed-cli whc_getbalance qpzua24x08gp2czcftm87zt3yut65ahn2g549g05jz 3
    {
      "balance": "100.00",
      "reserved": "0.00"
    }
6、空投

    #空投操作
    root@iZhp3gsd1ofz82zei2wx49Z:~/.bitcoin# wormholed-cli whc_sendsto qrkx59hgt29n980vsvkzj05ejay5en3905flhhhegl 3 100
    1db5d3b5c094e0d839465a1623d1b29baccb03e1bc2d817c6ee87d8b7ca4ebc4
    
    #空投后确认
    root@iZhp3gsd1ofz82zei2wx49Z:~/.bitcoin# wormholed-cli whc_getbalance qrkx59hgt29n980vsvkzj05ejay5en3905flhhhegl 3
    {
      "balance": "1160.64",
      "reserved": "0.00"
    }
    root@iZhp3gsd1ofz82zei2wx49Z:~/.bitcoin# wormholed-cli whc_getbalance qpzua24x08gp2czcftm87zt3yut65ahn2g549g05jz 3
    {
      "balance": "200.00",
      "reserved": "0.00"
    }
7、发送全部资产

    #发送前资产信息
    root@iZhp3gsd1ofz82zei2wx49Z:~/.bitcoin# wormholed-cli whc_getallbalancesforaddress qrkx59hgt29n980vsvkzj05ejay5en3905flhhhegl
    [
      {
        "propertyid": 1,
        "balance": "490.00000762",
        "reserved": "0.00000000"
      },
      {
        "propertyid": 3,
        "balance": "1160.64",
        "reserved": "0.00"
      }
    ]
    
    root@iZhp3gsd1ofz82zei2wx49Z:~/.bitcoin# wormholed-cli whc_getallbalancesforaddress qpzua24x08gp2czcftm87zt3yut65ahn2g549g05jz
    [
      {
        "propertyid": 1,
        "balance": "508.99999237",
        "reserved": "0.00000000"
      },
      {
        "propertyid": 3,
        "balance": "200.00",
        "reserved": "0.00"
      }
    ]
    
    #开发发送
    root@iZhp3gsd1ofz82zei2wx49Z:~/.bitcoin# wormholed-cli whc_sendall qpzua24x08gp2czcftm87zt3yut65ahn2g549g05jz qrkx59hgt29n980vsvkzj05ejay5en3905flhhhegl 1
    6b6b04aed7ed091a9b1f89b9517064cb16cd5032f1b0994d93b6db2a5d20e871
    
    #发送后资产确认
    root@iZhp3gsd1ofz82zei2wx49Z:~/.bitcoin# wormholed-cli whc_getallbalancesforaddress qpzua24x08gp2czcftm87zt3yut65ahn2g549g05jz
    [
    ]
    root@iZhp3gsd1ofz82zei2wx49Z:~/.bitcoin# wormholed-cli whc_getallbalancesforaddress qrkx59hgt29n980vsvkzj05ejay5en3905flhhhegl
    [
      {
        "propertyid": 1,
        "balance": "998.99999999",
        "reserved": "0.00000000"
      },
      {
        "propertyid": 3,
        "balance": "1360.64",
        "reserved": "0.00"
      }
    ]
    
四、创建管理资产
    
1、创建资产
    
    #创建资产操作
    root@iZhp3gsd1ofz82zei2wx49Z:~/.bitcoin# wormholed-cli whc_sendissuancemanaged qrkx59hgt29n980vsvkzj05ejay5en3905flhhhegl 1 8 0 "qshuai testnet create managed issuance" "first test" "managed issuance" "https://github.com/bcext/" "hello world"
    7a42d544775aedd38c67fda3c20ba80af1385a9b65cb8e95e85024ec1732e3d4
    
    #查看交易信息（确认后）
    root@iZhp3gsd1ofz82zei2wx49Z:~# wormholed-cli whc_gettransaction 7a42d544775aedd38c67fda3c20ba80af1385a9b65cb8e95e85024ec1732e3d4
    {
      "txid": "7a42d544775aedd38c67fda3c20ba80af1385a9b65cb8e95e85024ec1732e3d4",
      "fee": "6480",
      "sendingaddress": "bchreg:qrkx59hgt29n980vsvkzj05ejay5en3905flhhhegl",
      "ismine": true,
      "version": 0,
      "type_int": 54,
      "type": "Create Property - Manual",
      "propertyid": 4,
      "precision": "8",
      "ecosystem": "main",
      "category": "qshuai testnet create managed issuance",
      "subcategory": "first test",
      "propertyname": "managed issuance",
      "data": "hello world",
      "url": "https://github.com/bcext/",
      "amount": "0.00000000",
      "valid": true,
      "blockhash": "3f6087c556ec1ce261a286cec0434ca29215204f9e8394b98807639907115875",
      "blocktime": 1533465005,
      "positioninblock": 1,
      "block": 134,
      "confirmations": 1
    }

    #查看资产信息
    root@iZhp3gsd1ofz82zei2wx49Z:~# wormholed-cli whc_getproperty 4
    {
      "propertyid": 4,
      "name": "managed issuance",
      "category": "qshuai testnet create managed issuance",
      "subcategory": "first test",
      "data": "hello world",
      "url": "https://github.com/bcext/",
      "precision": 8,
      "issuer": "bchreg:qrkx59hgt29n980vsvkzj05ejay5en3905flhhhegl",
      "creationtxid": "7a42d544775aedd38c67fda3c20ba80af1385a9b65cb8e95e85024ec1732e3d4",
      "fixedissuance": false,
      "managedissuance": true,
      "freezingenabled": false,
      "totaltokens": "0.00000000"
    }

2、资产增发

    #增发操作
    root@iZhp3gsd1ofz82zei2wx49Z:~# wormholed-cli whc_sendgrant qrkx59hgt29n980vsvkzj05ejay5en3905flhhhegl "" 4 10000 "hello grant"
    1d0afab3383dfab315b7e3e2a5ba78d4ddb5481b8094843132e9134a4d589afc
    
    #查询交易信息（确认后）
    root@iZhp3gsd1ofz82zei2wx49Z:~# wormholed-cli whc_gettransaction 1d0afab3383dfab315b7e3e2a5ba78d4ddb5481b8094843132e9134a4d589afc
    {
      "txid": "1d0afab3383dfab315b7e3e2a5ba78d4ddb5481b8094843132e9134a4d589afc",
      "fee": "4700",
      "sendingaddress": "bchreg:qrkx59hgt29n980vsvkzj05ejay5en3905flhhhegl",
      "referenceaddress": "bchreg:qrkx59hgt29n980vsvkzj05ejay5en3905flhhhegl",
      "ismine": true,
      "version": 0,
      "type_int": 55,
      "type": "Grant Property Tokens",
      "propertyid": 4,
      "precision": "8",
      "amount": "10000.00000000",
      "valid": true,
      "blockhash": "1a56bf55c4a137303d8d8424f1dddb96ce3c2cfb1369cb98ac50748642d99ce8",
      "blocktime": 1533469110,
      "positioninblock": 1,
      "block": 135,
      "confirmations": 1
    }
    
    #查询资产信息
    root@iZhp3gsd1ofz82zei2wx49Z:~# wormholed-cli whc_getproperty 4
    {
      "propertyid": 4,
      "name": "managed issuance",
      "category": "qshuai testnet create managed issuance",
      "subcategory": "first test",
      "data": "hello world",
      "url": "https://github.com/bcext/",
      "precision": 8,
      "issuer": "bchreg:qrkx59hgt29n980vsvkzj05ejay5en3905flhhhegl",
      "creationtxid": "7a42d544775aedd38c67fda3c20ba80af1385a9b65cb8e95e85024ec1732e3d4",
      "fixedissuance": false,
      "managedissuance": true,
      "freezingenabled": false,
      "totaltokens": "10000.00000000"
    }
    
3、资产销毁

    #销毁操作
    root@iZhp3gsd1ofz82zei2wx49Z:~# wormholed-cli whc_sendrevoke qrkx59hgt29n980vsvkzj05ejay5en3905flhhhegl 4 200
    74b8d70c1a8f2d9b95cea883f5a6adac1ffc421e3dea984ef79373bddb47364c
    
    #资产明细
    root@iZhp3gsd1ofz82zei2wx49Z:~# wormholed-cli whc_getproperty 4
    {
      "propertyid": 4,
      "name": "managed issuance",
      "category": "qshuai testnet create managed issuance",
      "subcategory": "first test",
      "data": "hello world",
      "url": "https://github.com/bcext/",
      "precision": 8,
      "issuer": "bchreg:qrkx59hgt29n980vsvkzj05ejay5en3905flhhhegl",
      "creationtxid": "7a42d544775aedd38c67fda3c20ba80af1385a9b65cb8e95e85024ec1732e3d4",
      "fixedissuance": false,
      "managedissuance": true,
      "freezingenabled": false,
      "totaltokens": "9800.00000000"
    }
4、变更资产发行者

    #变更操作
    root@iZhp3gsd1ofz82zei2wx49Z:~# wormholed-cli whc_sendchangeissuer qrkx59hgt29n980vsvkzj05ejay5en3905flhhhegl qpzua24x08gp2czcftm87zt3yut65ahn2g549g05jz 4
    65fbd27bd1219b6c820aff9c43b2e175530cb0af0bbf14532fff132888af4906
    
    #查询资产信息（确认后）
    root@iZhp3gsd1ofz82zei2wx49Z:~# wormholed-cli whc_getproperty 4
    {
      "propertyid": 4,
      "name": "managed issuance",
      "category": "qshuai testnet create managed issuance",
      "subcategory": "first test",
      "data": "hello world",
      "url": "https://github.com/bcext/",
      "precision": 8,
      "issuer": "bchreg:qpzua24x08gp2czcftm87zt3yut65ahn2g549g05jz",
      "creationtxid": "7a42d544775aedd38c67fda3c20ba80af1385a9b65cb8e95e85024ec1732e3d4",
      "fixedissuance": false,
      "managedissuance": true,
      "freezingenabled": false,
      "totaltokens": "9800.00000000"
    }
5、转账

    #转账前余额
    root@iZhp3gsd1ofz82zei2wx49Z:~# wormholed-cli whc_getbalance qpzua24x08gp2czcftm87zt3yut65ahn2g549g05jz 4
    {
      "balance": "0.00000000",
      "reserved": "0.00000000"
    }
    root@iZhp3gsd1ofz82zei2wx49Z:~# wormholed-cli whc_getbalance qrkx59hgt29n980vsvkzj05ejay5en3905flhhhegl 4
    {
      "balance": "9800.00000000",
      "reserved": "0.00000000"
    }
    
    #开始转账
    root@iZhp3gsd1ofz82zei2wx49Z:~# wormholed-cli whc_send qrkx59hgt29n980vsvkzj05ejay5en3905flhhhegl qpzua24x08gp2czcftm87zt3yut65ahn2g549g05jz 4 800
    902bd9fb3ccc876c3c85dfefd9d87f41e8b3a4f1030edba5fc943ad848eccf61
    
    #查询交易信息（确认后）
    root@iZhp3gsd1ofz82zei2wx49Z:~# wormholed-cli whc_send qrkx59hgt29n980vsvkzj05ejay5en3905flhhhegl qpzua24x08gp2czcftm87zt3yut65ahn2g549g05jz 4 800
    902bd9fb3ccc876c3c85dfefd9d87f41e8b3a4f1030edba5fc943ad848eccf61
    
    #转账后余额
    root@iZhp3gsd1ofz82zei2wx49Z:~# wormholed-cli whc_getbalance qpzua24x08gp2czcftm87zt3yut65ahn2g549g05jz 4
    {
      "balance": "800.00000000",
      "reserved": "0.00000000"
    }
    root@iZhp3gsd1ofz82zei2wx49Z:~# wormholed-cli whc_getbalance qrkx59hgt29n980vsvkzj05ejay5en3905flhhhegl 4
    {
      "balance": "9000.00000000",
      "reserved": "0.00000000"
    }
    
5、空投
    
    #空投操作
    root@iZhp3gsd1ofz82zei2wx49Z:~# wormholed-cli whc_sendsto qrkx59hgt29n980vsvkzj05ejay5en3905flhhhegl 4 1000
    0cb5559698144baf90a2014193fef276dd7fe20d271344f5b0a32f8470cf8e58
    
    #查询交易信息（确认后）
    root@iZhp3gsd1ofz82zei2wx49Z:~# wormholed-cli whc_gettransaction 0cb5559698144baf90a2014193fef276dd7fe20d271344f5b0a32f8470cf8e58
    {
      "txid": "0cb5559698144baf90a2014193fef276dd7fe20d271344f5b0a32f8470cf8e58",
      "fee": "4540",
      "sendingaddress": "bchreg:qrkx59hgt29n980vsvkzj05ejay5en3905flhhhegl",
      "ismine": true,
      "version": 0,
      "type_int": 3,
      "type": "Send To Owners",
      "propertyid": 4,
      "precision": "8",
      "amount": "1000.00000000",
      "valid": true,
      "blockhash": "5a0343993375ab75bfc855cc6729a36c04ea4a9ff0dae3548778be7807ffd629",
      "blocktime": 1533469949,
      "positioninblock": 1,
      "block": 139,
      "confirmations": 1
    }
    
    #空投后确认余额
    root@iZhp3gsd1ofz82zei2wx49Z:~# wormholed-cli whc_getbalance qpzua24x08gp2czcftm87zt3yut65ahn2g549g05jz 4
    {
      "balance": "1800.00000000",
      "reserved": "0.00000000"
    }
    root@iZhp3gsd1ofz82zei2wx49Z:~# wormholed-cli whc_getbalance qrkx59hgt29n980vsvkzj05ejay5en3905flhhhegl 4
    {
      "balance": "8000.00000000",
      "reserved": "0.00000000"
    }
6、发送全部资产

    #发送前资产信息
    root@iZhp3gsd1ofz82zei2wx49Z:~# wormholed-cli whc_getallbalancesforaddress qrkx59hgt29n980vsvkzj05ejay5en3905flhhhegl
    [
      {
        "propertyid": 1,
        "balance": "997.99999998",
        "reserved": "0.00000000"
      },
      {
        "propertyid": 3,
        "balance": "1360.64",
        "reserved": "0.00"
      },
      {
        "propertyid": 4,
        "balance": "8000.00000000",
        "reserved": "0.00000000"
      }
    ]
    root@iZhp3gsd1ofz82zei2wx49Z:~# wormholed-cli whc_getallbalancesforaddress qpzua24x08gp2czcftm87zt3yut65ahn2g549g05jz
    [
      {
        "propertyid": 4,
        "balance": "1800.00000000",
        "reserved": "0.00000000"
      }
    ]
    
    #开始发送
    root@iZhp3gsd1ofz82zei2wx49Z:~# wormholed-cli whc_sendall qpzua24x08gp2czcftm87zt3yut65ahn2g549g05jz qrkx59hgt29n980vsvkzj05ejay5en3905flhhhegl 1
    34fb6eec252b28438b15a1f7feff63904a1784ccff343e034914ab856b96ace3
    
    #发送后资产确认
    root@iZhp3gsd1ofz82zei2wx49Z:~# wormholed-cli whc_getallbalancesforaddress qrkx59hgt29n980vsvkzj05ejay5en3905flhhhegl
    [
      {
        "propertyid": 1,
        "balance": "997.99999998",
        "reserved": "0.00000000"
      },
      {
        "propertyid": 3,
        "balance": "1360.64",
        "reserved": "0.00"
      },
      {
        "propertyid": 4,
        "balance": "9800.00000000",
        "reserved": "0.00000000"
      }
    ]
    root@iZhp3gsd1ofz82zei2wx49Z:~# wormholed-cli whc_getallbalancesforaddress qpzua24x08gp2czcftm87zt3yut65ahn2g549g05jz
    [
    ]
五、创建固定数量资产

1、创建资产

    #创建操作
    root@iZhp3gsd1ofz82zei2wx49Z:~# wormholed-cli whc_sendissuancefixed qrkx59hgt29n980vsvkzj05ejay5en3905flhhhegl 1 8 0 "qshuai testnet create fixed issuance" "first test" "fixed issuance" "https://github.com/bcext/" "hello world" 10000
    acec40a5ca1efed57a311b7dbb9324e753e7f5dd2395b3825163672b5edfc809
    
    #查询交易信息（确认后）
    root@iZhp3gsd1ofz82zei2wx49Z:~# wormholed-cli whc_gettransaction acec40a5ca1efed57a311b7dbb9324e753e7f5dd2395b3825163672b5edfc809
    {
      "txid": "acec40a5ca1efed57a311b7dbb9324e753e7f5dd2395b3825163672b5edfc809",
      "fee": "6560",
      "sendingaddress": "bchreg:qrkx59hgt29n980vsvkzj05ejay5en3905flhhhegl",
      "ismine": true,
      "version": 0,
      "type_int": 50,
      "type": "Create Property - Fixed",
      "propertyid": 5,
      "precision": "8",
      "ecosystem": "main",
      "category": "qshuai testnet create fixed issuance",
      "subcategory": "first test",
      "propertyname": "fixed issuance",
      "data": "hello world",
      "url": "https://github.com/bcext/",
      "amount": "10000.00000000",
      "valid": true,
      "blockhash": "7693fcae7c4877c05d95b4f5355795c96f39587893521106d99b6e6091093fd3",
      "blocktime": 1533470364,
      "positioninblock": 2,
      "block": 142,
      "confirmations": 1
    }
    
    #查询资产信息
    root@iZhp3gsd1ofz82zei2wx49Z:~# wormholed-cli whc_getproperty 5
    {
      "propertyid": 5,
      "name": "fixed issuance",
      "category": "qshuai testnet create fixed issuance",
      "subcategory": "first test",
      "data": "hello world",
      "url": "https://github.com/bcext/",
      "precision": 8,
      "issuer": "bchreg:qrkx59hgt29n980vsvkzj05ejay5en3905flhhhegl",
      "creationtxid": "acec40a5ca1efed57a311b7dbb9324e753e7f5dd2395b3825163672b5edfc809",
      "fixedissuance": true,
      "managedissuance": false,
      "totaltokens": "10000.00000000"
    }
    
2、变更资产发行者

    #变更操作
    root@iZhp3gsd1ofz82zei2wx49Z:~# wormholed-cli whc_sendchangeissuer qrkx59hgt29n980vsvkzj05ejay5en3905flhhhegl qpzua24x08gp2czcftm87zt3yut65ahn2g549g05jz 5
    730448d81c533db8f06203b183ae876b7a383db7ef15bf79660f60581389a379
    
    #查询资产信息
    root@iZhp3gsd1ofz82zei2wx49Z:~# wormholed-cli whc_getproperty 5
    {
      "propertyid": 5,
      "name": "fixed issuance",
      "category": "qshuai testnet create fixed issuance",
      "subcategory": "first test",
      "data": "hello world",
      "url": "https://github.com/bcext/",
      "precision": 8,
      "issuer": "bchreg:qpzua24x08gp2czcftm87zt3yut65ahn2g549g05jz",
      "creationtxid": "acec40a5ca1efed57a311b7dbb9324e753e7f5dd2395b3825163672b5edfc809",
      "fixedissuance": true,
      "managedissuance": false,
      "totaltokens": "10000.00000000"
    }
3、转账
    
    #转账前资产确认
    {
      "balance": "10000.00000000",
      "reserved": "0.00000000"
    }
    root@iZhp3gsd1ofz82zei2wx49Z:~# wormholed-cli whc_getbalance qpzua24x08gp2czcftm87zt3yut65ahn2g549g05jz 5
    {
      "balance": "0.00000000",
      "reserved": "0.00000000"
    }
    
    #开始转账
    root@iZhp3gsd1ofz82zei2wx49Z:~# wormholed-cli whc_send qrkx59hgt29n980vsvkzj05ejay5en3905flhhhegl qpzua24x08gp2czcftm87zt3yut65ahn2g549g05jz 5 1000
    8a8569653931ca2cc5c887ce3522fe7a500a071a006b1ddf2fc5cb0fc36e4bb3
    
    #转账后余额确认
    root@iZhp3gsd1ofz82zei2wx49Z:~# wormholed-cli whc_getbalance qrkx59hgt29n980vsvkzj05ejay5en3905flhhhegl 5
    {
      "balance": "9000.00000000",
      "reserved": "0.00000000"
    }
    root@iZhp3gsd1ofz82zei2wx49Z:~# wormholed-cli whc_getbalance qpzua24x08gp2czcftm87zt3yut65ahn2g549g05jz 5
    {
      "balance": "1000.00000000",
      "reserved": "0.00000000"
    }
4、空投

    #空投操作
    root@iZhp3gsd1ofz82zei2wx49Z:~# wormholed-cli whc_sendsto qrkx59hgt29n980vsvkzj05ejay5en3905flhhhegl 5 1000
    b3fe12d9f68c2b8d481877e341d93a999c798c8af48fe92ea5d7961f0f6c3bd9
    
    #空投后余额确认
    root@iZhp3gsd1ofz82zei2wx49Z:~# wormholed-cli whc_getbalance qrkx59hgt29n980vsvkzj05ejay5en3905flhhhegl 5
    {
      "balance": "8000.00000000",
      "reserved": "0.00000000"
    }
    root@iZhp3gsd1ofz82zei2wx49Z:~# wormholed-cli whc_getbalance qpzua24x08gp2czcftm87zt3yut65ahn2g549g05jz 5
    {
      "balance": "2000.00000000",
      "reserved": "0.00000000"
    }
5、发送全部资产

    #发送前资产确认
    root@iZhp3gsd1ofz82zei2wx49Z:~# wormholed-cli whc_getallbalancesforaddress qpzua24x08gp2czcftm87zt3yut65ahn2g549g05jz
    [
      {
        "propertyid": 5,
        "balance": "2000.00000000",
        "reserved": "0.00000000"
      }
    ]
    root@iZhp3gsd1ofz82zei2wx49Z:~# wormholed-cli whc_getallbalancesforaddress qrkx59hgt29n980vsvkzj05ejay5en3905flhhhegl
    [
      {
        "propertyid": 1,
        "balance": "996.99999997",
        "reserved": "0.00000000"
      },
      {
        "propertyid": 3,
        "balance": "1360.64",
        "reserved": "0.00"
      },
      {
        "propertyid": 4,
        "balance": "9800.00000000",
        "reserved": "0.00000000"
      },
      {
        "propertyid": 5,
        "balance": "8000.00000000",
        "reserved": "0.00000000"
      }
    ]
    
    #开始发送
    root@iZhp3gsd1ofz82zei2wx49Z:~# wormholed-cli whc_sendall qpzua24x08gp2czcftm87zt3yut65ahn2g549g05jz qrkx59hgt29n980vsvkzj05ejay5en3905flhhhegl 1
    e6ec9dd0d385abf1c7fd7033c5092cd4f74ad9c52303bab705e3f44c500f2a93
    
    #发送后资产确认
    root@iZhp3gsd1ofz82zei2wx49Z:~# wormholed-cli whc_getallbalancesforaddress qrkx59hgt29n980vsvkzj05ejay5en3905flhhhegl
    [
      {
        "propertyid": 1,
        "balance": "996.99999997",
        "reserved": "0.00000000"
      },
      {
        "propertyid": 3,
        "balance": "1360.64",
        "reserved": "0.00"
      },
      {
        "propertyid": 4,
        "balance": "9800.00000000",
        "reserved": "0.00000000"
      },
      {
        "propertyid": 5,
        "balance": "10000.00000000",
        "reserved": "0.00000000"
      }
    ]
    root@iZhp3gsd1ofz82zei2wx49Z:~# wormholed-cli whc_getallbalancesforaddress qpzua24x08gp2czcftm87zt3yut65ahn2g549g05jz
    [
    ]

