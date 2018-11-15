## Version upgrade changes

1. Add new RPC interface
2. Use decimal string to represent the ERC721 PropertyID and ERC721 TokenID in RPC interface.
3. Add check conditions to the RPC interface.



## Warning

This version does not involve changes to the Wormhole consensus rule, and does not require a mandatory upgrade if you are currently running on a node version of v0.2.1.



## Bitcoin-Abc compatible

The `Wormhole 0.2.2` node is compatible with the `Bitcoin-Abc 0.18.2` version. The Wormhole node can also be used as a Bitcoin-Abc node to support all functions of the Bitcoin-Abc node.



## Get the Wormhole node version

```
wormholed-cli whc_getinfo
{  
  "wormholeversion_int": 20002000,
  "wormholeversion": "0.2.2",
  "bitcoincoreversion": "0.18.2",
  "block": 1266612,
  "blocktime": 1541556523,
  "blocktransactions": 0,
  "totaltransactions": 5155,
  "alerts": [
  ]
}
```

Wormhole node version : `"wormholeversion": "0.2.2"`

Bitcoin-Abc version : `"bitcoincoreversion": "0.18.2"`

The Wormhole version released in this document is 0.2.2 and Bitcoin-ABC 0.18.2 is supported.



## How to upgrade

1. Download the code for version 0.2.2：https://github.com/copernet/wormhole/releases/tag/v0.2.2
2. Install, compile
   - Unix platform：https://github.com/copernet/wormhole/blob/master/doc/build-unix.md
   - OSX platform：https://github.com/copernet/wormhole/blob/master/doc/build-osx.md
   - Windows platform：https://github.com/copernet/wormhole/blob/master/doc/build-windows.md
3. Run the version 0.2.2 using the following command for the first time：`wormholed -startclean=1 -daemon`
4. Use the following command when the software is restarted the next time after the data synchronization is completed for version 0.2.2：`wormholed -daemon`



### ERC721 function enable height

Mainnet：555655

Testnet：1267112

Regtest：110



### Add RPC interfaces for ERC721

See RPC's detailed explanation：https://github.com/copernet/spec/blob/master/wormhole-RPC.md



#### whc_listERC721PropertyTokens

Description：Lists all ERC721Token that specify the ERC721 property issuance



### Modify part of the RPC interface

1. The representation of the asset ID and TokenID in the parameters and return values of the RPC interface is reformatted from hexadecimal to decimal.
2. Add check conditions to the RPC interface where the transaction was created, reducing invalid Wormhole transactions.



Example:

```
wormholed-cli whc_getERC721TokenNews 2 65536
{
  "propertyid": "2",
  "tokenid": "65536",
  "owner": "bchtest:qz04wg2jj75x34tge2v8w0l6r0repfcvcygv3t7sg5",
  "creationtxid": "dfe02400f88a27ace9586b7a9dfbcdaa585a99fd78e23c5ec84b8b428ed87d42",
  "creationblock": "000000000000005b25d7b2f924f4cd7c97d5412a5f0b174ceabd15e4f951cc55",
  "attribute": "0000000000000000000000000000000000000000000000000000000000000001",
  "tokenurl": "test"
}
```



#### whc_gettransaction

Description: Get information about wormhole transactions

#### whc_getERC721PropertyNews

Description: Get the information of ERC721 property

#### whc_getERC721TokenNews

Description: Get the information of ERC721 Token

#### whc_getERC721AddressTokens

Description: Get Token in the specified address under the specified ERC asset

#### whc_getERC721PropertyDestroyTokens

Description: Get the destroyed ERC721 Tokens under the specified ERC721property

#### whc_issuanceERC721property

Description: Issuance of ERC721 property

#### whc_issuanceERC721Token

Description: Issuance of ERC721 Token

#### whc_transferERC721Token

Description: Transfer of ERC721 Token

#### whc_destroyERC721Token

Description: Destruction of ERC721 Token

#### 

## Wormhole Spec documents

1. White Paper     https://github.com/copernet/spec/blob/master/whcwhitepaper-en.pdf
2. Yellow Paper     https://github.com/copernet/spec/blob/master/wormhole-yellowpaper-en.md
3. Spec       https://github.com/copernet/spec/blob/master/wormhole-spec-en.md
4. RPC manual    https://github.com/copernet/spec/blob/master/wormhole-rpc-en.md
5. Test manual   https://github.com/copernet/spec/blob/master/wormhole-testmanual-0.2.2-en.md



