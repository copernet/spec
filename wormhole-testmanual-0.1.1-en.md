Test manual of wormhole version 0.1.1

一、Test environment build

1. Software download：https://github.com/copernet/wormhole/releases/tag/Earth-0.1.1-release

2. Compile and Installation

   Unix platform：https://github.com/copernet/wormhole/blob/master/doc/build-unix.md

   OSX platform：https://github.com/copernet/wormhole/blob/master/doc/build-osx.md

   Windows platform：https://github.com/copernet/wormhole/blob/master/doc/build-windows.md

3. Run and data synchronization

   Run the version 0.1.1 using the following command for the first time：`wormholed -startclean=1 -daemon`

   Use the following command when the software is restarted the next time after the data synchronization is completed for version 0.1.1：`wormholed -daemon`

二、Basic environmental preparation

Base environment preparation includes address generation, WHC acquisition.

As shown in the https://github.com/copernet/spec/blob/master/wormhole-testmanual-0.0.6.md

三、crowdfunding testing

- Issuer a crowdfunding transaction

```
wormholed-cli whc_sendissuancecrowdsale qpfgew6hes5k9e40mzcngz86hrvc786f8ycu9snrva 1 3 0 "hello crowdsale test" "cr test" "haha" "www" "hehe" 1 "100" 39408120741 10 0 "10000"
```

- view transaction info

```
wormholed-cli whc_gettransaction f20c6c8f2bba3cde7729d05f6a683081cdddcec32375be5f0b37042d610d2e8d
{
  "txid": "f20c6c8f2bba3cde7729d05f6a683081cdddcec32375be5f0b37042d610d2e8d",
  "fee": "319",
  "sendingaddress": "bchtest:qpfgew6hes5k9e40mzcngz86hrvc786f8ycu9snrva",
  "ismine": true,
  "version": 0,
  "type_int": 51,
  "type": "Create Property - Variable",
  "propertyid": 416,
  "precision": "3",
  "ecosystem": "main",
  "category": "hello crowdsale test",
  "subcategory": "cr test",
  "propertyname": "haha",
  "data": "hehe",
  "url": "www",
  "propertyiddesired": 1,
  "tokensperunit": "100.00000000",
  "deadline": 39408120741,
  "earlybonus": 10,
  "amount": "10000.000",
  "valid": true,
  "blockhash": "00000000000002eebdf93757217549cb1f381b34b640696ebe74971dc77c8718",
  "blocktime": 1539778631,
  "positioninblock": 2,
  "block": 1263047,
  "confirmations": 1
}
```

- Participate crowdfunding

```
wormholed-cli whc_particrowsale qpwyl4vh9np38qeqnu9t5zsn057yvvxv8cf3a7ezvg qpfgew6hes5k9e40mzcngz86hrvc786f8ycu9snrva 1
9958c62bdeb95c2bad0c8218e104570e74266b0d0461005854f5e09ae522a443
```

- View transaction

```
wormholed-cli whc_gettransaction 9958c62bdeb95c2bad0c8218e104570e74266b0d0461005854f5e09ae522a443
{
  "txid": "9958c62bdeb95c2bad0c8218e104570e74266b0d0461005854f5e09ae522a443",
  "fee": "281",
  "sendingaddress": "bchtest:qpwyl4vh9np38qeqnu9t5zsn057yvvxv8cf3a7ezvg",
  "referenceaddress": "bchtest:qpfgew6hes5k9e40mzcngz86hrvc786f8ycu9snrva",
  "ismine": true,
  "version": 0,
  "type_int": 1,
  "type": "Crowdsale Purchase",
  "propertyid": 1,
  "precision": "8",
  "amount": "1.00000000",
  "actualInvested": "0.01596857",
  "purchasedpropertyid": 416,
  "purchasedpropertyname": "haha",
  "purchasedpropertyprecision": "3",
  "purchasedtokens": "10000.000",
  "issuertokens": "0.000",
  "valid": true,
  "blockhash": "00000000000004b18051490fce33ee2dad84e7c9be49f01f7636767b0c9dab9d",
  "blocktime": 1539779638,
  "positioninblock": 3,
  "block": 1263049,
  "confirmations": 2
}
```

- View balances

```
wormholed-cli whc_getbalance qpwyl4vh9np38qeqnu9t5zsn057yvvxv8cf3a7ezvg 416
{
  "balance": "10000.000",
  "reserved": "0.000"
}
wormholed-cli whc_getbalance qpfgew6hes5k9e40mzcngz86hrvc786f8ycu9snrva 416
{
  "balance": "0.000",
  "reserved": "0.000"
}
```

- The maximum amount of crowdfunding reached, it will be automatically closed

```
wormholed-cli whc_particrowsale qpwyl4vh9np38qeqnu9t5zsn057yvvxv8cf3a7ezvg qpfgew6hes5k9e40mzcngz86hrvc786f8ycu9snrva 1
1fd70648a9506f015a0aab227b4d56ca5854fa3b4dfec45cff1ba22a518143f3

wormholed-cli whc_gettransaction 1fd70648a9506f015a0aab227b4d56ca5854fa3b4dfec45cff1ba22a518143f3
{
  "txid": "1fd70648a9506f015a0aab227b4d56ca5854fa3b4dfec45cff1ba22a518143f3",
  "fee": "281",
  "sendingaddress": "bchtest:qpwyl4vh9np38qeqnu9t5zsn057yvvxv8cf3a7ezvg",
  "referenceaddress": "bchtest:qpfgew6hes5k9e40mzcngz86hrvc786f8ycu9snrva",
  "ismine": true,
  "version": 0,
  "type_int": 1,
  "type": "Crowdsale Purchase",
  "valid": false,
  "invalidreason": "the receive address no have active crowd property",
  "blockhash": "000000000000055611336f6b1bfca3ac68e7ea34a6c466e0bbc10a7d901f6c3d",
  "blocktime": 1539780486,
  "positioninblock": 21,
  "block": 1263052,
  "confirmations": 1
}
```

- Issue a new crowdfunding

```
 wormholed-cli whc_sendissuancecrowdsale qpfgew6hes5k9e40mzcngz86hrvc786f8ycu9snrva 1 3 0 "hello Mr.liu crowdsale test" "crc test" "haha" "www" "hehehe" 1 "100" 1539784088 10 0 "10000"
be2d54048899eff3504f2a4c41645412bda6d960146d607e45320b6d23179900

 wormholed-cli whc_gettransaction be2d54048899eff3504f2a4c41645412bda6d960146d607e45320b6d23179900
{
  "txid": "be2d54048899eff3504f2a4c41645412bda6d960146d607e45320b6d23179900",
  "fee": "330",
  "sendingaddress": "bchtest:qpfgew6hes5k9e40mzcngz86hrvc786f8ycu9snrva",
  "ismine": true,
  "version": 0,
  "type_int": 51,
  "type": "Create Property - Variable",
  "propertyid": 418,
  "precision": "3",
  "ecosystem": "main",
  "category": "hello Mr.liu crowdsale test",
  "subcategory": "crc test",
  "propertyname": "haha",
  "data": "hehehe",
  "url": "www",
  "propertyiddesired": 1,
  "tokensperunit": "100.00000000",
  "deadline": 1539784088,
  "earlybonus": 10,
  "amount": "10000.000",
  "valid": true,
  "blockhash": "0000000000000130f6c842e481e420aea1f70b9864fe15ad4b4c6c3426817d3e",
  "blocktime": 1539781086,
  "positioninblock": 24,
  "block": 1263053,
  "confirmations": 1
}
```

- Participate crowdfunding

```
wormholed-cli whc_particrowsale qpwyl4vh9np38qeqnu9t5zsn057yvvxv8cf3a7ezvg qpfgew6hes5k9e40mzcngz86hrvc786f8ycu9snrva 1
872fc95eef0857ab0233fd493b5050a8afa1e2ba61edf4348f60e28284640e3e

wormholed-cli whc_gettransaction 872fc95eef0857ab0233fd493b5050a8afa1e2ba61edf4348f60e28284640e3e
{
  "txid": "872fc95eef0857ab0233fd493b5050a8afa1e2ba61edf4348f60e28284640e3e",
  "fee": "281",
  "sendingaddress": "bchtest:qpwyl4vh9np38qeqnu9t5zsn057yvvxv8cf3a7ezvg",
  "referenceaddress": "bchtest:qpfgew6hes5k9e40mzcngz86hrvc786f8ycu9snrva",
  "ismine": true,
  "version": 0,
  "type_int": 1,
  "type": "Crowdsale Purchase",
  "propertyid": 1,
  "precision": "8",
  "amount": "1.00000000",
  "actualInvested": "0.99999305",
  "purchasedpropertyid": 418,
  "purchasedpropertyname": "haha",
  "purchasedpropertyprecision": "3",
  "purchasedtokens": "100.029",
  "issuertokens": "0.000",
  "valid": true,
  "blockhash": "00000000d6ac8d3faf4061e79dd270272b0aafef1e28671e44b05f697dd113f4",
  "blocktime": 1539782292,
  "positioninblock": 42,
  "block": 1263054,
  "confirmations": 2
}
```

- View balance

```
/wormholed-cli whc_getbalance qpwyl4vh9np38qeqnu9t5zsn057yvvxv8cf3a7ezvg 418
{
  "balance": "100.029",
  "reserved": "0.000"
}
wormholed-cli whc_getbalance qpfgew6hes5k9e40mzcngz86hrvc786f8ycu9snrva 418
{
  "balance": "0.000",
  "reserved": "0.000"
}
```

- Check the issuer balance after the deadline

```
wormholed-cli whc_getbalance qpfgew6hes5k9e40mzcngz86hrvc786f8ycu9snrva 418
{
  "balance": "9899.971",
  "reserved": "0.000"
}
```

- Issue a new crowdfunding

```
wormholed-cli whc_sendissuancecrowdsale qpfgew6hes5k9e40mzcngz86hrvc786f8ycu9snrva 1 3 0 "hello Mr.liu, morning crowdsale test " "crc test" "haha" "www" "hehehe" 1 "100" 1539834061 10 0 "10000"
8d1a6293c130964eb2a227f3b6c471264d94ea88451b8d74079ea04fdb373c3d

wormholed-cli whc_gettransaction 8d1a6293c130964eb2a227f3b6c471264d94ea88451b8d74079ea04fdb373c3d
{
  "txid": "8d1a6293c130964eb2a227f3b6c471264d94ea88451b8d74079ea04fdb373c3d",
  "fee": "342",
  "sendingaddress": "bchtest:qpfgew6hes5k9e40mzcngz86hrvc786f8ycu9snrva",
  "ismine": true,
  "version": 0,
  "type_int": 51,
  "type": "Create Property - Variable",
  "propertyid": 425,
  "precision": "3",
  "ecosystem": "main",
  "category": "hello Mr.liu, morning crowdsale test ",
  "subcategory": "crc test",
  "propertyname": "haha",
  "data": "hehehe",
  "url": "www",
  "propertyiddesired": 1,
  "tokensperunit": "100.00000000",
  "deadline": 1539834061,
  "earlybonus": 10,
  "amount": "10000.000",
  "valid": true,
  "blockhash": "0000000000000585ec5e0a55805346ba4d9d195136bf4a0581a02c5a4f8541bf",
  "blocktime": 1539831574,
  "positioninblock": 46,
  "block": 1263154,
  "confirmations": 2
}
```

- Participate crowdfunding

```
wormholed-cli whc_particrowsale qpwyl4vh9np38qeqnu9t5zsn057yvvxv8cf3a7ezvg qpfgew6hes5k9e40mzcngz86hrvc786f8ycu9snrva 1
fb53aee4bbc73b0c66c3fde8fb0f0b7927a62fa02c416633f5a39a8712dedb9f

wormholed-cli whc_gettransaction fb53aee4bbc73b0c66c3fde8fb0f0b7927a62fa02c416633f5a39a8712dedb9f
{
  "txid": "fb53aee4bbc73b0c66c3fde8fb0f0b7927a62fa02c416633f5a39a8712dedb9f",
  "fee": "282",
  "sendingaddress": "bchtest:qpwyl4vh9np38qeqnu9t5zsn057yvvxv8cf3a7ezvg",
  "referenceaddress": "bchtest:qpfgew6hes5k9e40mzcngz86hrvc786f8ycu9snrva",
  "ismine": true,
  "version": 0,
  "type_int": 1,
  "type": "Crowdsale Purchase",
  "propertyid": 1,
  "precision": "8",
  "amount": "1.00000000",
  "actualInvested": "0.99999823",
  "purchasedpropertyid": 425,
  "purchasedpropertyname": "haha",
  "purchasedpropertyprecision": "3",
  "purchasedtokens": "100.038",
  "issuertokens": "0.000",
  "valid": true,
  "blockhash": "00000000000000253be84f68f92119ee907227c1b98e0c15b07ea7669874efa5",
  "blocktime": 1539831752,
  "positioninblock": 2,
  "block": 1263156,
  "confirmations": 1
}
```

- View balance

```
wormholed-cli whc_getbalance qpwyl4vh9np38qeqnu9t5zsn057yvvxv8cf3a7ezvg 425
{
  "balance": "100.038",
  "reserved": "0.000"
}
wormhole/src# ./wormholed-cli whc_getbalance qpfgew6hes5k9e40mzcngz86hrvc786f8ycu9snrva 425
{
  "balance": "0.000",
  "reserved": "0.000"
}
```

- Close the crowdfunding manual

```
wormholed-cli whc_sendclosecrowdsale qpfgew6hes5k9e40mzcngz86hrvc786f8ycu9snrva 425
7ded167e8c8226360820a21763ff4b2b5d8cf2cf2b65025cf951ada6c391455c

wormholed-cli whc_gettransaction 7ded167e8c8226360820a21763ff4b2b5d8cf2cf2b65025cf951ada6c391455c
{
  "txid": "7ded167e8c8226360820a21763ff4b2b5d8cf2cf2b65025cf951ada6c391455c",
  "fee": "236",
  "sendingaddress": "bchtest:qpfgew6hes5k9e40mzcngz86hrvc786f8ycu9snrva",
  "ismine": true,
  "version": 0,
  "type_int": 53,
  "type": "Close Crowdsale",
  "propertyid": 425,
  "precision": "3",
  "valid": true,
  "blockhash": "00000000000002c0bd031790479b3d20bde5210e68f01db223a0472cbdc552a4",
  "blocktime": 1539833121,
  "positioninblock": 10,
  "block": 1263158,
  "confirmations": 1
}
```

- View balance

```
wormholed-cli whc_getbalance qpwyl4vh9np38qeqnu9t5zsn057yvvxv8cf3a7ezvg 425
{
  "balance": "100.038",
  "reserved": "0.000"
}
wormholed-cli whc_getbalance qpfgew6hes5k9e40mzcngz86hrvc786f8ycu9snrva 425
{
  "balance": "9899.962",
  "reserved": "0.000"
}
```