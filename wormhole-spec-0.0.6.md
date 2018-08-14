## 版本升级变动
1. 修改了对外RPC接口中 WHC的单位
    *   0.0.6以前，RPC的部分接口，对于WHC金额的显示，以C为单位
    *   0.0.6版本中，所有RPC的接口，对于WHC金额的显示，以`WHC`为单位
    *   1WHC = 1 * 10<sup>8</sup> C
2. 开放了众筹功能
3. 修复了0.0.5中BUG: `由于节点重启，导致数据混乱的问题`
4. 增加了一个RPC，获取指定地址的活跃众筹: `whc_getactivecrowd`


## 如何升级
1. 下载0.0.6版本的代码：https://github.com/copernet/wormhole/releases/tag/Earth-0.0.6-pre-release
2. 安装，编译
    * Unix平台：https://github.com/copernet/wormhole/blob/master/doc/build-unix.md
    * OSX平台：https://github.com/copernet/wormhole/blob/master/doc/build-osx.md
    * Windows平台：https://github.com/copernet/wormhole/blob/master/doc/build-windows.md
2. 初次运行0.0.6版本的代码，使用如下命令：`wormholed -startclean=1 -daemon`
3. 当0.0.6版本启动，且数据同步完成后，下次软件重启时，使用如下命令：`wormholed -daemon`

## RPC接口的变动

### whc_getallbalancesforaddress
返回值中：WHC的金额以`WHC`为单位。

### whc_getallbalancesforid
返回值中：WHC的金额以`WHC`为单位。

### whc_getbalance
返回值中：WHC的金额以`WHC`为单位。

### whc_getcrowdsale
返回值中：WHC的金额以`WHC`为单位。

### whc_getproperty
返回值中：WHC的金额以`WHC`为单位。

### whc_getsto
返回值中：WHC的金额以`WHC`为单位。

### whc_getactivecrowd
增加了该RPC接口；

### whc_gettransaction
返回值中：WHC的金额以`WHC`为单位。

## 修复的BUG
commit news：https://github.com/copernet/wormhole/commit/fcee4574a5f4113ade5b3dbceca6f868f135b33a

