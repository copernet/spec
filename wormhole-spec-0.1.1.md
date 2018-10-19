## 版本升级变动

1. 优化了RPC接口中的金额计算方式

2. 为众筹创建交易设置总金额检查高度，检查内容：总金额不允许大于INT64_MAX

3. 修复BUG：众筹结束后，剩余token未发送给发行者

4. 增加众筹集成测试

## 如何升级

1. 下载0.1.1版本的代码：https://github.com/copernet/wormhole/releases/tag/Earth-0.1.1-release
2. 安装，编译
   - Unix平台：https://github.com/copernet/wormhole/blob/master/doc/build-unix.md
   - OSX平台：https://github.com/copernet/wormhole/blob/master/doc/build-osx.md
   - Windows平台：https://github.com/copernet/wormhole/blob/master/doc/build-windows.md
3. 初次运行0.1.1版本的代码，使用如下命令，同步数据：`wormholed -startclean=1 -daemon`
4. 当0.1.1版本同步数据完成后，下次软件重启时，使用如下命令：`wormholed -daemon`

## 众筹总金额检查

wormhole v0.1.1将在块高度552892起对众筹总金额进行检查，其范围必续在0到INT64_MAX之间，不包括0。

## 修复众筹剩余资产未发送至发行者的BUG

在0.1.1 版本中修复该BUG

- commit信息：https://github.com/copernet/wormhole/commit/af2a2f2556516affcb39a9585c351adda5b0c1a4
- 该BUG导致的情况：当一笔众筹结束后，剩余的未售出token没有发送到众筹发起人的地址上。

## Wormhole Spec 文档

1. 白皮书     https://github.com/copernet/spec/blob/master/whcwhitepaper.md
2. 黄皮书     https://github.com/copernet/spec/blob/master/Wormhole-YellowPaper.md
3. spec       https://github.com/copernet/spec/blob/master/wormhole-spec.md
4. rpc手册    https://github.com/copernet/spec/blob/master/wormhole-RPC.md
5. 测试手册   https://github.com/copernet/spec/blob/master/wormhole-testmanual-0.1.1.md

