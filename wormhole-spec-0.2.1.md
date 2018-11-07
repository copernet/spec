## 版本升级变动

1. 增加 RPC查询接口
2. 修复ERC721 相关的issue



## 强烈建议

该版本修复了 ERC721相关的issue，涉及到Wormhole 规则的变化，所有节点必须强制升级至本次发布的版本(0.2.1)。同时将不再支持 0.2.1 版本以下的节点，旧节点将由于共识规则的变化，无法提供准确的 Wormhole交易信息。



## Bitcoin-Abc 节点兼容

`Wormhole 0.2.1` 节点兼容 `Bitcoin-Abc 0.18.2`版本，Wormhole节点 也可以被用来作为Bitcoin-Abc 节点，支持Bitcoin-Abc 节点的所有功能。



## 获取 Wormhole  节点版本

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

Wormhole 节点版本 : `"wormholeversion": "0.2.1"`

Bitcoin-Abc 版本 : `"bitcoincoreversion": "0.18.2"`



本文档发布的 Wormhole 版本 为 0.2.1；支持 Bitcoin-ABC 0.18.2 版本.



## 如何升级

1. 下载0.2.1版本的代码：https://github.com/copernet/wormhole/releases/tag/v0.2.1
2. 安装，编译
   - Unix平台：https://github.com/copernet/wormhole/blob/master/doc/build-unix.md
   - OSX平台：https://github.com/copernet/wormhole/blob/master/doc/build-osx.md
   - Windows平台：https://github.com/copernet/wormhole/blob/master/doc/build-windows.md
3. 初次运行0.2.1版本的代码，使用如下命令，同步数据：`wormholed -startclean=1 -daemon`
4. 当0.2.1版本同步数据完成后，下次软件重启时，使用如下命令：`wormholed -daemon`



### ERC721 功能的启用高度

主网启用高度： 555655

测试网启用高度：1267112

回归测试网启用高度：110



### 增加的RPC

RPC的详细解释见：https://github.com/copernet/spec/blob/master/wormhole-RPC.md

### whc_ownerOfERC721Token

描述：查询该Token的所有者 是否为指定地址



## Wormhole Spec 文档

1. 白皮书     https://github.com/copernet/spec/blob/master/whcwhitepaper.md
2. 黄皮书     https://github.com/copernet/spec/blob/master/wormhole-yellow-paper.md
3. spec       https://github.com/copernet/spec/blob/master/wormhole-spec.md
4. rpc手册    https://github.com/copernet/spec/blob/master/wormhole-RPC.md
5. 测试手册   https://github.com/copernet/spec/blob/master/wormhole-testmanual-0.2.0.md

