## 版本升级变动
1. 修改部分RPC接口
2. 修复RPC接口的BUG
3. 增加集成测试
4. WHC 的Token精度
5. 增加gitian构建


## 如何升级
1. 下载0.1.0版本的代码：https://github.com/copernet/wormhole/releases/tag/Earth-0.1.0-release
2. 安装，编译
    * Unix平台：https://github.com/copernet/wormhole/blob/master/doc/build-unix.md
    * OSX平台：https://github.com/copernet/wormhole/blob/master/doc/build-osx.md
    * Windows平台：https://github.com/copernet/wormhole/blob/master/doc/build-windows.md
2. 初次运行0.1.0版本的代码，使用如下命令，同步数据：`wormholed -startclean=1 -daemon`
3. 当0.1.0版本同步数据完成后，下次软件重启时，使用如下命令：`wormholed -daemon`

## RPC接口的变动
### whc_gettransaction
对于参与众筹交易
*   增加了一个字段`actualInvested`，标识众筹实际花费的WHC金额
*   原`amount`字段，标识参与众筹输入的WHC金额

### 修改参与众筹RPC名称拼写错误
0.1.0 版本前；RPC名称为：`whc_createpayload_particrwosale`
0.1.0 版本； RPC名称为：`whc_createpayload_particrowdsale`


## 修复RPC接口的BUG
### whc_decodetransaction
在0.1.0 版本中修复该RPC接口的BUG；
    * commit信息：https://github.com/copernet/wormhole/commit/59b4cef865765d4fcfc340c32893185492eb5810
    * 该BUG导致的情况：当调用该RPC解码一个错误的BCH交易时，由于数组越界，造成节点宕机。

## WHC 的Token精度
WHC 的token精度 由0变为8.

## Wormhole Spec 文档
1. 白皮书     https://github.com/copernet/spec/blob/master/whcwhitepaper.md
2. 黄皮书     https://github.com/copernet/spec/blob/master/Wormhole-YellowPaper.md
3. spec       https://github.com/copernet/spec/blob/master/wormhole-spec.md
4. rpc手册    https://github.com/copernet/spec/blob/master/wormhole-RPC.md
5. 测试手册   https://github.com/copernet/spec/blob/master/wormhole-testmanual-0.0.6.md
    * `参与众筹RPC`的名称修改，其余部分，与0.0.6的测试文档通用。

