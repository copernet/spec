## 版本升级变动

- 兼容[bitcoin-abc v0.20.6](https://github.com/Bitcoin-ABC/bitcoin-abc/releases/tag/v0.20.6)

## Bitcoin-Abc 节点兼容

- `Wormhole v0.2.5`兼容`Bitcoin-abc v0.20.6`版本，Wormhole节点支持Bitcoin-Abc的所有功能。

## 获取 Wormhole 节点版本

```
wormholed-cli whc_getinfo
{  
  "wormholeversion_int": 20005000,
  "wormholeversion": "0.2.5",
  "bitcoincoreversion": "0.20.6",
  "block": 	609828,
  "blocktime": 1574233677,
  "blocktransactions": 137,
  "totaltransactions": 5155,
  "alerts": [
  ]
}
```

Wormhole 节点版本 : "wormholeversion": "0.2.5"<br>
Bitcoin-Abc 版本 : "bitcoincoreversion": "0.20.6"<br>
本文档发布的 Wormhole 版本 为 0.2.5；支持 Bitcoin-ABC 0.20.6 版本.<br>

## 如何升级

1. 下载0.2.5版本的代码：https://github.com/copernet/wormhole/releases/tag/v0.2.5
2. 安装，编译
    *   Unix平台：https://github.com/copernet/wormhole/blob/master/doc/build-unix.md
    *   OSX平台：https://github.com/copernet/wormhole/blob/master/doc/build-osx.md
    *   Windows平台：https://github.com/copernet/wormhole/blob/master/doc/build-windows.md
3. 初次运行0.2.5版本的代码，使用如下命令，同步数据：wormholed -startclean=1 -daemon
4. 当0.2.5版本同步数据完成后，下次软件重启时，使用如下命令：wormholed -daemon

## Wormhole Spec 文档

1. 白皮书 https://github.com/copernet/spec/blob/master/whcwhitepaper.md
2. 黄皮书 https://github.com/copernet/spec/blob/master/wormhole-yellow-paper.md
3. spec https://github.com/copernet/spec/blob/master/wormhole-spec.md
4. rpc手册 https://github.com/copernet/spec/blob/master/wormhole-RPC.md





