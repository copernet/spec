## Version Upgrade
1.	Change some RPC interfaces
2.	Fix bugs of RPC interfaces
3.	Add integration testing
4.	Improve property precision
5.	Add gitian construction 

## How to upgrade
1.	Download the code of Version 0.01 : https://github.com/copernet/wormhole/releases/tag/Earth-0.1.0-release
2.	Install，compile
    * Unix Platform : https://github.com/copernet/wormhole/blob/master/doc/build-unix.md
    * OSX Platform : https://github.com/copernet/wormhole/blob/master/doc/build-osx.md
    * Windows Platform : https://github.com/copernet/wormhole/blob/master/doc/build-windows.md
3.	When you firstly use the code of version 0.01, you can run the following commands to synchronize data `wormholed -startclean=1 -daemon`
4.	After you finish synchronizing data of version 0.01, you can run the following commands while reboot next time `wormholed -daemon`


## Change the RPC interface

### Whc-gettransaction

Crowdfunding transaction
* Add a field `actualInvested` to identify the amount of WHC spent in crowdfunding 
* The Original field `amount` is to identify the entering amount of WHC involved in crowdfunding



### Change the spelling error of RPC involved in crowdingfunding 
Before Version 0.1.0: RPC was named as `whc_createpayload_particrwosale`

Version 0.1.0: RPC is named as `whc_createpayload_particrowdsale`
 
 
## Modify the bug of RPC interface

### Whc-decodetransaction

Modify the bugs of RPC interface within Version 0.01
* Commit information : https://github.com/copernet/wormhole/commit/59b4cef865765d4fcfc340c32893185492eb5810 
* Existed bugs may cause the following situation: when the RRC is called to decode a wrong BCH transaction, it will make nodes crash due to array bounds


## WHC token precision
The WHC token precision has changed from 0 to 8

## Wormhole Spec document

1.	White paper : https://github.com/copernet/spec/blob/master/whcwhitepaper.md
2.	Yellow paper : https://github.com/copernet/spec/blob/master/Wormhole-YellowPaper.md
3.	Wormhole Spec :  https://github.com/copernet/spec/blob/master/wormhole-spec.md
4.	RPC-API : https://github.com/copernet/spec/blob/master/wormhole-RPC.md
5.	Test-Cases : https://github.com/copernet/spec/blob/master/wormhole-testmanual-0.0.6.md
    * We have changed the name of RPC involved in crowdfunding, the rest will follow the test book of Version 0.06 
 
