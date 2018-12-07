## 版本升级变动

1. 修复RPC `whc_gettransaction` 相关的问题
2. 修复因为`Bitcoin-ABC 0.18.2` 规则启动，导致回归测试网络创建区块失败的问题
3. 修改部分集成测试



## Bitcoin-Abc 节点兼容

`Wormhole 0.2.3` 节点兼容 `Bitcoin-Abc 0.18.2`版本，Wormhole节点 也可以被用来作为Bitcoin-Abc 节点，支持Bitcoin-Abc 节点的所有功能。



## 获取 Wormhole  节点版本

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

Wormhole 节点版本 : `"wormholeversion": "0.2.3"`

Bitcoin-Abc 版本 : `"bitcoincoreversion": "0.18.2"`



本文档发布的 Wormhole 版本 为 0.2.3；支持 Bitcoin-ABC 0.18.2 版本.



## 如何升级

1. 下载0.2.3版本的代码：https://github.com/copernet/wormhole/releases/tag/v0.2.3
2. 安装，编译
   - Unix平台：https://github.com/copernet/wormhole/blob/master/doc/build-unix.md
   - OSX平台：https://github.com/copernet/wormhole/blob/master/doc/build-osx.md
   - Windows平台：https://github.com/copernet/wormhole/blob/master/doc/build-windows.md
3. 初次运行0.2.3版本的代码，使用如下命令，同步数据：`wormholed -startclean=1 -daemon`
4. 当0.2.3版本同步数据完成后，下次软件重启时，使用如下命令：`wormholed -daemon`



## 修复的RPC的应答

RPC的详细解释见：https://github.com/copernet/spec/blob/master/wormhole-RPC.md



#### whc_gettransaction

**描述**：查询 Wormhole 交易的详细信息

**修复的问题**：

* 冻结/解冻 类型交易的应答增加用户金额字段 `amount` 
* 众筹交易查询信息不全



### 修复回归测试网创建区块失败的问题

**描述**：因为 `Bitcoin-ABC 0.18.2`共识规则生效后，区块中每个交易的大小必须最少为100 bytes。在回归测试网上，由于节点 生成的区块中 coinbase交易的字节数少于100字节，导致区块验证失败。



## Wormhole Spec 文档

1. 白皮书     https://github.com/copernet/spec/blob/master/whcwhitepaper.md
2. 黄皮书     https://github.com/copernet/spec/blob/master/wormhole-yellow-paper.md
3. spec       https://github.com/copernet/spec/blob/master/wormhole-spec.md
4. rpc手册    https://github.com/copernet/spec/blob/master/wormhole-RPC.md





