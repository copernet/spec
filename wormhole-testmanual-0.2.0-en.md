Test manual of wormhole version 0.2.0

一、Test environment build

1. Software download：https://github.com/copernet/wormhole/releases/tag/v0.2.0

2. Compile and Installation

   Unix platform：https://github.com/copernet/wormhole/blob/master/doc/build-unix.md

   OSX platform：https://github.com/copernet/wormhole/blob/master/doc/build-osx.md

   Windows platform：https://github.com/copernet/wormhole/blob/master/doc/build-windows.md

3. Run and data synchronization

   Run the version 0.2.0 using the following command for the first time：`wormholed -startclean=1 -daemon`

   Use the following command when the software is restarted the next time after the data synchronization is completed for version 0.2.0：`wormholed -daemon`

二、Basic environmental preparation

Base environment preparation includes address generation, WHC acquisition.

As shown in the https://github.com/copernet/spec/blob/master/wormhole-testmanual-0.0.6.md

三、Functional test

1. Issue ERC721 property 

```
wormholed-cli whc_issuanceERC721property bchreg:qq9py4f509ufykpvx556kt3fwyt40mx09q59q0adf2 name nihao ludete wormhole 999
c9a4ded68b8099b4fdb541fede3293f0d3c5920809c7876ea05f9f72ebba8233
```

2. Get transaction information

```
wormholed-cli whc_gettransaction c9a4ded68b8099b4fdb541fede3293f0d3c5920809c7876ea05f9f72ebba8233
{
  "txid": "c9a4ded68b8099b4fdb541fede3293f0d3c5920809c7876ea05f9f72ebba8233",
  "fee": "4940",
  "sendingaddress": "bchreg:qq9py4f509ufykpvx556kt3fwyt40mx09q59q0adf2",
  "referenceaddress": "bchreg:qq9py4f509ufykpvx556kt3fwyt40mx09q59q0adf2",
  "ismine": true,
  "version": 0,
  "type_int": 9,
  "type": "Issue ERC721 Property",
  "action": 1,
  "erc721propertyid": "000000000000000000000000000000000000000000000000000000000000000b",
  "valid": true,
  "blockhash": "584d370632e0070c6e8f23dc48e55e92697cfb2800b328a8eef8416239ed347d",
  "blocktime": 1541158298,
  "positioninblock": 2,
  "block": 730,
  "confirmations": 1
}
```

3. Get property information

```
wormholed-cli whc_getERC721PropertyNews 0x0b
{
  "propertyid": "000000000000000000000000000000000000000000000000000000000000000b",
  "owner": "bchreg:qq9py4f509ufykpvx556kt3fwyt40mx09q59q0adf2",
  "creationtxid": "c9a4ded68b8099b4fdb541fede3293f0d3c5920809c7876ea05f9f72ebba8233",
  "creationblock": "584d370632e0070c6e8f23dc48e55e92697cfb2800b328a8eef8416239ed347d",
  "name": "name",
  "symbol": "nihao",
  "data": "ludete",
  "propertyurl": "wormhole",
  "totalTokenNumber": 999,
  "haveIssuedNumber": 0,
  "currentValidIssuedNumer": 0
}
```

4. Issue ERC721 property token

```
wormholed-cli whc_issuanceERC721Token  qq9py4f509ufykpvx556kt3fwyt40mx09q59q0adf2 qp8xjgy9wjeu7zkn9zxm609wqk6rre76jqv6zupt0v 0x0b 0x01 0x253678 www.ludete.com
b18ac8cd6efb98a4ccbf9f96331e05851f370ea44193f63260737080b52bed77
```

5. Get transaction information

```
wormholed-cli whc_gettransaction b18ac8cd6efb98a4ccbf9f96331e05851f370ea44193f63260737080b52bed77
{
  "txid": "b18ac8cd6efb98a4ccbf9f96331e05851f370ea44193f63260737080b52bed77",
  "fee": "7160",
  "sendingaddress": "bchreg:qq9py4f509ufykpvx556kt3fwyt40mx09q59q0adf2",
  "referenceaddress": "bchreg:qp8xjgy9wjeu7zkn9zxm609wqk6rre76jqv6zupt0v",
  "ismine": true,
  "version": 0,
  "type_int": 9,
  "type": "Issue ERC721 Token",
  "action": 2,
  "erc721propertyid": "000000000000000000000000000000000000000000000000000000000000000b",
  "erc721tokenid": "0000000000000000000000000000000000000000000000000000000000000001",
  "valid": true,
  "blockhash": "28966c4ea419c17bff994738193b500bfa3c27942cfa05cee6a2feef68fd67c7",
  "blocktime": 1541168050,
  "positioninblock": 1,
  "block": 731,
  "confirmations": 1
}
```

6. Get token information

```
wormholed-cli whc_getERC721TokenNews 0x0b 0x01
{
  "propertyid": "000000000000000000000000000000000000000000000000000000000000000b",
  "tokenid": "0000000000000000000000000000000000000000000000000000000000000001",
  "owner": "bchreg:qp8xjgy9wjeu7zkn9zxm609wqk6rre76jqv6zupt0v",
  "creationtxid": "b18ac8cd6efb98a4ccbf9f96331e05851f370ea44193f63260737080b52bed77",
  "creationblock": "28966c4ea419c17bff994738193b500bfa3c27942cfa05cee6a2feef68fd67c7",
  "attribute": "0000000000000000000000000000000000000000000000000000000000253678",
  "tokenurl": "www.ludete.com"
}
```

7. Transfer ERC721 Token

```
wormholed-cli whc_transferERC721Token qp8xjgy9wjeu7zkn9zxm609wqk6rre76jqv6zupt0v qq9py4f509ufykpvx556kt3fwyt40mx09q59q0adf2 0x0b 0x01
8d02a8f940b5a3dae071d0a1af77c6c35525855cc45c773a47e9260bf31930ba
```

8. Get transaction information

```
wormholed-cli whc_gettransaction 8d02a8f940b5a3dae071d0a1af77c6c35525855cc45c773a47e9260bf31930ba
{
  "txid": "8d02a8f940b5a3dae071d0a1af77c6c35525855cc45c773a47e9260bf31930ba",
  "fee": "6200",
  "sendingaddress": "bchreg:qp8xjgy9wjeu7zkn9zxm609wqk6rre76jqv6zupt0v",
  "referenceaddress": "bchreg:qq9py4f509ufykpvx556kt3fwyt40mx09q59q0adf2",
  "ismine": true,
  "version": 0,
  "type_int": 9,
  "type": "Transfer ERc721 Token",
  "action": 3,
  "erc721propertyid": "000000000000000000000000000000000000000000000000000000000000000b",
  "erc721tokenid": "0000000000000000000000000000000000000000000000000000000000000001",
  "valid": true,
  "blockhash": "6230a257647d13fb7904d799f4f7b92f3944285756b412827a0def8ec538d8e5",
  "blocktime": 1541168567,
  "positioninblock": 1,
  "block": 735,
  "confirmations": 1
}
```

9. Get the ERC721 Token contained in the specified address under the specified property

```
wormholed-cli whc_getERC721AddressTokens qq9py4f509ufykpvx556kt3fwyt40mx09q59q0adf2 0x0b
[
  {
    "tokenid": "0000000000000000000000000000000000000000000000000000000000000001",
    "attribute": "0000000000000000000000000000000000000000000000000000000000253678",
    "tokenurl": "www.ludete.com",
    "creationtxid": "b18ac8cd6efb98a4ccbf9f96331e05851f370ea44193f63260737080b52bed77"
  }
]
```

10. Revoke ERC721 token

```
wormholed-cli whc_destroyERC721Token qq9py4f509ufykpvx556kt3fwyt40mx09q59q0adf2 0x0b 0x01
a8dcd8e3d741535e359e1ad41655a2945db26aa7a73f487825366d9ce8c860c7
```

11. Get transaction information

```
wormholed-cli whc_gettransaction a8dcd8e3d741535e359e1ad41655a2945db26aa7a73f487825366d9ce8c860c7
{
  "txid": "a8dcd8e3d741535e359e1ad41655a2945db26aa7a73f487825366d9ce8c860c7",
  "fee": "5520",
  "sendingaddress": "bchreg:qq9py4f509ufykpvx556kt3fwyt40mx09q59q0adf2",
  "referenceaddress": "bchreg:qq9py4f509ufykpvx556kt3fwyt40mx09q59q0adf2",
  "ismine": true,
  "version": 0,
  "type_int": 9,
  "type": "Destroy ERC721 Token",
  "action": 4,
  "erc721propertyid": "000000000000000000000000000000000000000000000000000000000000000b",
  "erc721tokenid": "0000000000000000000000000000000000000000000000000000000000000001",
  "valid": true,
  "blockhash": "26c8184bc2234b918c8c2a0bebe319c5d74f096fca21fc8299cbf3c337196606",
  "blocktime": 1541168789,
  "positioninblock": 1,
  "block": 736,
  "confirmations": 1
}
```

12. Get token information

```
wormholed-cli whc_getERC721TokenNews 0x0b 0x01
{
  "propertyid": "000000000000000000000000000000000000000000000000000000000000000b",
  "tokenid": "0000000000000000000000000000000000000000000000000000000000000001",
  "owner": "bchreg:pqqqqqqqqqqqqqqqqqqqqqqqqqqqqp0kvc457r8whc",
  "creationtxid": "b18ac8cd6efb98a4ccbf9f96331e05851f370ea44193f63260737080b52bed77",
  "creationblock": "28966c4ea419c17bff994738193b500bfa3c27942cfa05cee6a2feef68fd67c7",
  "attribute": "0000000000000000000000000000000000000000000000000000000000253678",
  "tokenurl": "www.ludete.com"
}
```

13. Get the ERC721 token that has been destroyed under the specified property

```
wormholed-cli whc_getERC721PropertyDestroyTokens 0x0b
[
  {
    "tokenid": "0000000000000000000000000000000000000000000000000000000000000001",
    "attribute": "0000000000000000000000000000000000000000000000000000000000253678",
    "tokenurl": "www.ludete.com",
    "creationtxid": "b18ac8cd6efb98a4ccbf9f96331e05851f370ea44193f63260737080b52bed77"
  }
]
```