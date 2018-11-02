## 版本升级变动

1. 实现ERC721 的功能
   * 发行ERC721 资产
   * 发行ERC721 Token
   * 转移ERC721 Token
   * 销毁ERC721 Token
2. 增加ERC721 的集成测试
3. 增加ERC721 的RPC接口
4. 修改了部分RPC接口

## 如何升级

1. 下载0.1.3版本的代码：https://github.com/copernet/wormhole/releases/tag/v0.1.3
2. 安装，编译
   - Unix平台：https://github.com/copernet/wormhole/blob/master/doc/build-unix.md
   - OSX平台：https://github.com/copernet/wormhole/blob/master/doc/build-osx.md
   - Windows平台：https://github.com/copernet/wormhole/blob/master/doc/build-windows.md
3. 初次运行0.1.3版本的代码，使用如下命令，同步数据：`wormholed -startclean=1 -daemon`
4. 当0.1.3版本同步数据完成后，下次软件重启时，使用如下命令：`wormholed -daemon`



### 实现ERC721 的功能

1. 新增加Wormhole 交易类型 : `WHC_TYPE_ERC721 (9)`，标识ERC721 交易.

2. 新增加 `ERC721Action` 枚举类型， 标识ERC721 交易中相关的操作.

   * ```
     enum ERC721Action{
         ISSUE_ERC721_PROPERTY = 1,
         ISSUE_ERC721_TOKEN,
         TRANSFER_REC721_TOKEN,
         DESTROY_ERC721_TOKEN
     };
     ```



### 增加ERC721 的集成测试

测试执行：

```
1. 前置条件：成功编译 Wormhole 项目
2. 进入目录：cd wormhole/test/functional
3. 测试运行：./test_runner.py whc_erc721.py
```



### 增加的RPC

RPC的详细解释见：https://github.com/copernet/spec/blob/master/wormhole-RPC.md

#### whc_issuanceERC721property

描述：发行ERC721 资产



#### whc_issuanceERC721Token

描述：发行ERC721 Token



#### whc_transferERC721Token

描述：转移ERC721 Token



#### whc_destroyERC721Token

描述：销毁ERC721 Token



#### whc_createpayload_issueERC721property

描述：生成 创建ERC721 资产的载荷数据



#### whc_createpayload_issueERC721token

描述：生成 创建ERC721 Token的载荷数据



#### whc_createpayload_transferERC721token

描述：生成转移ERC721 Token的载荷数据



#### whc_createpayload_destroyERC721token

描述：生成销毁ERC721 Token的载荷数据 



#### whc_getERC721PropertyNews

描述：获取ERC721 资产的信息



#### whc_getERC721TokenNews

描述：获取ERC721 Token的信息



#### whc_getERC721AddressTokens

描述：获取指定ERC资产下，指定地址含有的Token



#### whc_getERC721PropertyDestroyTokens

描述：获取指定资产中被销毁的ERC721 Token



### 修改的RPC接口

#### whc_gettransaction

描述：获取Wormhole 交易的信息

变动：

* 返回值中 增加了`erc721propertyid` 字段标识 ERC721 资产ID
* 返回值中 增加了 `erc721tokenid` 字段标识 ERC721 TokenID



## Wormhole Spec 文档

1. 白皮书     https://github.com/copernet/spec/blob/master/whcwhitepaper.md
2. 黄皮书     https://github.com/copernet/spec/blob/master/wormhole-yellow-paper.md
3. spec       https://github.com/copernet/spec/blob/master/wormhole-spec.md
4. rpc手册    https://github.com/copernet/spec/blob/master/wormhole-RPC.md
5. 测试手册   https://github.com/copernet/spec/blob/master/wormhole-testmanual-0.1.3.md





