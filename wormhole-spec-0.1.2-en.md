## Version upgrade changes

1. Add managed property freezing and unfreezing function
2. Modify the explanation of the *previous token ID* field in the issued managed property transaction

## How to upgrade

1. Download the code for version 0.1.2：https://github.com/copernet/wormhole/releases/tag/v0.1.2
2. Install, compile
   - Unix platform：https://github.com/copernet/wormhole/blob/master/doc/build-unix.md
   - OSX platform：https://github.com/copernet/wormhole/blob/master/doc/build-osx.md
   - Windows platform：https://github.com/copernet/wormhole/blob/master/doc/build-windows.md
3. Run the version 0.1.2 using the following command for the first time：`wormholed -startclean=1 -daemon`
4. Use the following command when the software is restarted the next time after the data synchronization is completed for version 0.1.2：`wormholed -daemon`

## Property freezing and unfreezing function

Version 0.1.2 adds the freezing and unfreezing function for managed property. The property issuer can freeze the specific token under the specified address, and the frozen address cannot transfer the token under it until the issuer unfreezing the token, and the issuer is not allowed to transfer the property manager identity to the frozen address. The newly added RPC interfaces are as follows:

**whc_createpayload_freeze**

Description：create wormhole freezing property payload

**whc_createpayload_unfreeze**

Description：create wormhole unfreezing property payload

**whc_sendfreeze**

Description：create and send wormhole freezing property transaction

**whc_sendunfreeze**

Description：create and send wormhole unfreezing property transaction

**whc_getfrozenbalance**

Description：To obtain the balance of a specific token and address

**whc_getfrozenbalanceforid**

Description：To obtain the balances of a specific token

**whc_getfrozenbalanceforaddress**

Description：To obtain the balances of a specific address

## Changes of the RPC interface

**whc_sendissuancemanaged**

Description：To issue a manageable property

Field change：previousid；

Version 0.1.2 reinterprets the *previousid* field. It represents the property freeze function switch with a legal value of 0,1now. The default value is 0 (compatible with previous versions, that is, turning off the freeze function), and the new increment value of 1 means turning on the property freeze function.

## Wormhole Spec documents

1. White Paper     https://github.com/copernet/spec/blob/master/whcwhitepaper-en.pdf
2. Yellow Paper     https://github.com/copernet/spec/blob/master/wormhole-yellowpaper-en.md
3. Spec       https://github.com/copernet/spec/blob/master/wormhole-spec-en.md
4. RPC manual    https://github.com/copernet/spec/blob/master/wormhole-rpc-en.md
5. Test manual   https://github.com/copernet/spec/blob/master/wormhole-testmanual-0.1.2-en.md

