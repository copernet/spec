## Version upgrade changes

1. Add new RPC interface
2. Fix some issue about ERC721 



## Warning

This version fixes the issue related to ERC721, which involves the change of the Wormhole rule, and all nodes must be forced to upgrade to the version released this time (0.2.1). At the same time, nodes below version 0.2.1 will no longer be supported, and old nodes will not be able to provide accurate Wormhole trading information due to changes in consensus rules.



## Bitcoin-Abc compatible

The `Wormhole 0.2.1` node is compatible with the `Bitcoin-Abc 0.18.2` version. The Wormhole node can also be used as a Bitcoin-Abc node to support all functions of the Bitcoin-Abc node.



## Get the Wormhole node version

```
wormholed-cli whc_getinfo
{  
  "wormholeversion_int": 20001000,
  "wormholeversion": "0.2.1",
  "bitcoincoreversion": "0.18.2",
  "block": 1266612,
  "blocktime": 1541556523,
  "blocktransactions": 0,
  "totaltransactions": 5155,
  "alerts": [
  ]
}
```

Wormhole node version : `"wormholeversion": "0.2.1"`

Bitcoin-Abc version : `"bitcoincoreversion": "0.18.2"`

The Wormhole version released in this document is 0.2.1 and Bitcoin-ABC 0.18.2 is supported.



## How to upgrade

1. Download the code for version 0.2.1：https://github.com/copernet/wormhole/releases/tag/v0.2.1
2. Install, compile
   - Unix platform：https://github.com/copernet/wormhole/blob/master/doc/build-unix.md
   - OSX platform：https://github.com/copernet/wormhole/blob/master/doc/build-osx.md
   - Windows platform：https://github.com/copernet/wormhole/blob/master/doc/build-windows.md
3. Run the version 0.2.1 using the following command for the first time：`wormholed -startclean=1 -daemon`
4. Use the following command when the software is restarted the next time after the data synchronization is completed for version 0.2.1：`wormholed -daemon`



### ERC721 function enable height

Mainnet：555655

Testnet：1267112

Regtest：110



### Add RPC interfaces for ERC721

See RPC's detailed explanation：https://github.com/copernet/spec/blob/master/wormhole-RPC.md



### whc_ownerOfERC721Token

Description：Query whether the Token's owner is the specified address



## Wormhole Spec documents

1. White Paper     https://github.com/copernet/spec/blob/master/whcwhitepaper-en.pdf
2. Yellow Paper     https://github.com/copernet/spec/blob/master/wormhole-yellowpaper-en.md
3. Spec       https://github.com/copernet/spec/blob/master/wormhole-spec-en.md
4. RPC manual    https://github.com/copernet/spec/blob/master/wormhole-rpc-en.md
5. Test manual   https://github.com/copernet/spec/blob/master/wormhole-testmanual-0.2.0-en.md

