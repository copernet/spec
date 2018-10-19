## Version upgrade change

- Modify the calculation method of property amount for RPCs.

- Set the height to start total amount check,  that the total amount is not allowed to be greater than INT64_MAX, for establishment of crowdfunding.

- Fix the bug that the remaining tokens not sent to the issuer after the crowdfunding close. 

- Add some of the crowdfunding to the Integration testing
- Fix confirmations informations in  RPC whc_decodetransaction
-  Append the amount field to the return of RPC whc_gettransaction,  identify how many WHCs user input to participate the crowdfunding,  for unconfirmed crowdfunding participate transaction

## How to Upgrade

1. Software download：https://github.com/copernet/wormhole/releases/tag/Earth-0.1.1-release

2. Compile and Installation

   Unix platform：https://github.com/copernet/wormhole/blob/master/doc/build-unix.md

   OSX platform：https://github.com/copernet/wormhole/blob/master/doc/build-osx.md

   Windows platform：https://github.com/copernet/wormhole/blob/master/doc/build-windows.md

3. Run and data synchronization

   Run the version 0.1.1 using the following command for the first time：`wormholed -startclean=1 -daemon`

   Use the following command when the software is restarted the next time after the data synchronization is completed for version 0.1.1：`wormholed -daemon`

## Check the total amount of crowdfunding

Wormhole v0.1.1 will check the total amount of crowdfunding from block height of 552,892, which must be between 0 and INT64_MAX, excluding 0.

## Fix the BUG 

Fix the BUG that the remaining properties of crowdfunding are not sent to the issuer in version 0.1.1

- commit info：https://github.com/copernet/wormhole/commit/af2a2f2556516affcb39a9585c351adda5b0c1a4
- The situation caused by this BUG: when a crowdfunding is finished, the remaining unsold token is not sent to the address of the issuer.

## Wormhole Spec Docs

1. white paper     https://github.com/copernet/spec/blob/master/whcwhitepaper.md
2. Yellpw paper     https://github.com/copernet/spec/blob/master/Wormhole-YellowPaper.md
3. spec       https://github.com/copernet/spec/blob/master/wormhole-spec.md
4. rpc manual   https://github.com/copernet/spec/blob/master/wormhole-RPC.md
5. test manual   https://github.com/copernet/spec/blob/master/wormhole-testmanual-0.1.1.md