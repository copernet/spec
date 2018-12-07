## Version upgrade changes

1. Fix RPC `whc_gettransaction` issue
2. Fix block creation failure on regtest network due to the activation of new rules in 'Bitcoin-ABC 0.18.2'
3. Modify some integration tests 



## Bitcoin-Abc compatible

The `Wormhole 0.2.3` node is compatible with the `Bitcoin-Abc 0.18.2` version. The Wormhole node can also be used as a Bitcoin-Abc node to support all functions of the Bitcoin-Abc node.



## Get the Wormhole node version

```
wormholed-cli whc_getinfo
{  
  "wormholeversion_int": 20003000,
  "wormholeversion": "0.2.3",
  "bitcoincoreversion": "0.18.2",
  "block": 1269612,
  "blocktime": 1541556523,
  "blocktransactions": 0,
  "totaltransactions": 5155,
  "alerts": [
  ]
}
```

Wormhole node version : `"wormholeversion": "0.2.3"`

Bitcoin-Abc version : `"bitcoincoreversion": "0.18.2"`

The Wormhole version released in this document is 0.2.3 and Bitcoin-ABC 0.18.2 is supported.



## How to upgrade

1. Download the code for version 0.2.3：https://github.com/copernet/wormhole/releases/tag/v0.2.3
2. Install, compile
   - Unix platform：https://github.com/copernet/wormhole/blob/master/doc/build-unix.md
   - OSX platform：https://github.com/copernet/wormhole/blob/master/doc/build-osx.md
   - Windows platform：https://github.com/copernet/wormhole/blob/master/doc/build-windows.md
3. Run the version 0.2.3 using the following command for the first time：`wormholed -startclean=1 -daemon`
4. Use the following command when the software is restarted the next time after the data synchronization is completed for version 0.2.3：`wormholed -daemon`

### 

## Fixed RPC reply

See RPC's detailed explanation：https://github.com/copernet/spec/blob/master/wormhole-RPC.md



#### whc_gettransaction

**Description**: Get information about wormhole transactions

**Fixed issues**:

* Query information of crowdfunding transactions is incompleted
* Add field ` amount` for Frozen/Unfrozen transaction



#### Fix block creation failure on regtest network

**Description**: The consensus requires that the minmal size of transactions in blocks is 100 bytes after `Bitcoin-ABC 0.18.2` activation.  So the block is invalid because the size of coinbase transaction is less than 100 bytes in regression network.



## Wormhole Spec documents

1. White Paper     https://github.com/copernet/spec/blob/master/whcwhitepaper-en.pdf
2. Yellow Paper     https://github.com/copernet/spec/blob/master/wormhole-yellowpaper-en.md
3. Spec       https://github.com/copernet/spec/blob/master/wormhole-spec-en.md
4. RPC manual    https://github.com/copernet/spec/blob/master/wormhole-rpc-en.md



##  