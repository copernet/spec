## 版本升级变动

1. 增加 RPC接口
2. 重新格式化ERC721资产ID与TokenID的表示形式
3. 在创建交易的 RPC 接口实现中添加 检查条件


## Bitcoin-Abc 节点兼容

`Wormhole 0.2.2` 节点兼容 `Bitcoin-Abc 0.18.2`版本，Wormhole节点 也可以被用来作为Bitcoin-Abc 节点，支持Bitcoin-Abc 节点的所有功能。



## 获取 Wormhole  节点版本

```
wormholed-cli whc_getinfo
{  
  "wormholeversion_int": 20002000,
  "wormholeversion": "0.2.2",
  "bitcoincoreversion": "0.18.2",
  "block": 1266612,
  "blocktime": 1541556523,
  "blocktransactions": 0,
  "totaltransactions": 5155,
  "alerts": [
  ]
}
```

Wormhole 节点版本 : `"wormholeversion": "0.2.2"`

Bitcoin-Abc 版本 : `"bitcoincoreversion": "0.18.2"`



本文档发布的 Wormhole 版本 为 0.2.2；支持 Bitcoin-ABC 0.18.2 版本.



## 如何升级

1. 下载0.2.2版本的代码：https://github.com/copernet/wormhole/releases/tag/v0.2.2
2. 安装，编译
   - Unix平台：https://github.com/copernet/wormhole/blob/master/doc/build-unix.md
   - OSX平台：https://github.com/copernet/wormhole/blob/master/doc/build-osx.md
   - Windows平台：https://github.com/copernet/wormhole/blob/master/doc/build-windows.md
3. 初次运行0.2.2版本的代码，使用如下命令，同步数据：`wormholed -startclean=1 -daemon`
4. 当0.2.2版本同步数据完成后，下次软件重启时，使用如下命令：`wormholed -daemon`



### ERC721 功能的启用高度

主网启用高度： 555655

测试网启用高度：1267112

回归测试网启用高度：110



### 增加的RPC

RPC的详细解释见：https://github.com/copernet/spec/blob/master/wormhole-RPC.md



#### whc_listERC721PropertyTokens

描述：列出指定ERC721 资产发行的所有ERC721Token



### RPC接口的变动

1. 将RPC接口的 接收参数和返回值中 资产ID和TokenID的 表现形式从16进制 重新格式化为10进制。
2. 在创建交易的RPC接口中添加检查条件，减少无效的Wormhole交易。

示例如下：

```
wormholed-cli whc_getERC721TokenNews 2 65536
{
  "propertyid": "2",
  "tokenid": "65536",
  "owner": "bchtest:qz04wg2jj75x34tge2v8w0l6r0repfcvcygv3t7sg5",
  "creationtxid": "dfe02400f88a27ace9586b7a9dfbcdaa585a99fd78e23c5ec84b8b428ed87d42",
  "creationblock": "000000000000005b25d7b2f924f4cd7c97d5412a5f0b174ceabd15e4f951cc55",
  "attribute": "0000000000000000000000000000000000000000000000000000000000000001",
  "tokenurl": "test"
}
```



#### whc_gettransaction

描述：获取Wormhole 交易的详细信息



#### whc_getERC721PropertyNews

描述：获取ERC721资产的详细信息



#### whc_getERC721TokenNews

描述：获取ERC721Token的详细信息



#### whc_getERC721AddressTokens

描述：获取指定ERC资产下，指定地址含有的Token



#### whc_getERC721PropertyDestroyTokens

描述：获取指定资产中被销毁的ERC721 Token



#### whc_issuanceERC721property

描述：发行ERC721 资产



#### whc_issuanceERC721Token

描述：发行ERC721 Token



#### whc_transferERC721Token

描述：转移ERC721 Token



#### whc_destroyERC721Token

描述：销毁ERC721 Token



## Wormhole Spec 文档

1. 白皮书     https://github.com/copernet/spec/blob/master/whcwhitepaper.md
2. 黄皮书     https://github.com/copernet/spec/blob/master/wormhole-yellow-paper.md
3. spec       https://github.com/copernet/spec/blob/master/wormhole-spec.md
4. rpc手册    https://github.com/copernet/spec/blob/master/wormhole-RPC.md
5. 测试手册   https://github.com/copernet/spec/blob/master/wormhole-testmanual-0.2.2.md







