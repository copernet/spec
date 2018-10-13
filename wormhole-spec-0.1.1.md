## 版本升级变动

1. 增加可管理资产冻结与解冻功能
2. 修改发行可管理资产交易中previous token ID字段解释

## 如何升级

1. 下载0.1.1版本的代码：https://github.com/copernet/wormhole/releases/tag/Earth-0.1.1-release
2. 安装，编译
   - Unix平台：https://github.com/copernet/wormhole/blob/master/doc/build-unix.md
   - OSX平台：https://github.com/copernet/wormhole/blob/master/doc/build-osx.md
   - Windows平台：https://github.com/copernet/wormhole/blob/master/doc/build-windows.md
3. 初次运行0.1.1版本的代码，使用如下命令，同步数据：`wormholed -startclean=1 -daemon`
4. 当0.1.1版本同步数据完成后，下次软件重启时，使用如下命令：`wormholed -daemon`

## 资产冻结解冻功能

0.1.1版本增加可管理资产的冻结与解冻功能，资产发行者可以冻结指定地址下的资产，被冻结地址不能转移其下资产直到发行者解冻其资产，并且发行者不允许将资产管理者身份转移给冻结地址。新增RPC接口如下：

**whc_createpayload_freeze**

解释：构建wormhole冻结资产交易载荷

**whc_createpayload_unfreeze**

解释：构建wormhole解冻资产交易载荷

**whc_sendfreeze**

解释：构建并发送wormhole冻结资产交易

**whc_sendunfreeze**

解释：构建并发送wormhole解冻资产交易

**whc_getfrozenbalance**

解释：按地址与资产ID查询wormhole冻结资产余额

**whc_getfrozenbalanceforid**

解释：按资产ID查询wormhole冻结资产余额

**whc_getfrozenbalanceforaddress**

解释：按地址查询wormhole冻结资产余额

## 版本变动RPC接口

**whc_sendissuancemanaged**

描述：发行可管理的token

变动字段：previousid；

版本对previousid字段进行重新解释，当前版本表示资产冻结功能开关，合法值为0，1。默认为0（兼容之前版本，即关闭冻结功能），新增值1表示开启资产冻结功能。

## Wormhole Spec 文档

1. 白皮书     https://github.com/copernet/spec/blob/master/whcwhitepaper.md
2. 黄皮书     https://github.com/copernet/spec/blob/master/Wormhole-YellowPaper.md
3. spec       https://github.com/copernet/spec/blob/master/wormhole-spec.md
4. rpc手册    https://github.com/copernet/spec/blob/master/wormhole-RPC.md
5. 测试手册   https://github.com/copernet/spec/blob/master/wormhole-testmanual-0.1.1.md

