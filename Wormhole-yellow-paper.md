

## 简介
`Bitcoin Cash`（BCH）在区块高度478,558上产生，一直致力于为世界带来一种可靠的电子现金，履行最初的比特币作为「点对点数字现金」的承诺。其具有全球无缝流通、无许可（Permissionless)创新等特点。在`Bitcoin Cash`如何实现发行通证（Token），众多的开发者已经有过不少的研究，比如染色币的方案[Colored-Coins](https://github.com/Colored-Coins/Colored-Coins-Protocol-Specification/wiki)，之后Andrew Stone 提出了[Enable representative tokens via OP_GROUP on Bitcoin Cash](https://github.com/BitcoinUnlimited/BUIP/blob/master/077.mediawiki)，提议增加OP_GROUP的操作码来实现发Token的方案。OP_GROUP方案需要修改Bitcoin Cash的共识规则才可以实现。更具体地说，类似于在Ethereum网络上广受欢迎的ERC20协议所具备的那些功能。

凡是需要更改共识才能实现的通证发行技术提议，都不可避免地会遇到问题。首先是技术上的风险，其次是对这种风险的顾虑常常引发技术开发社区甚至整个经济生态都陷入巨大的争议。争议中的反对方，其顾虑很可能也确实是真实的。不论这样的争议中谁对谁错，结果常常是有争议的提议无法被实现。这样的困难可以被视为一种保险机制，让具有的风险更改很难被添加到协议之中，保证协议的稳健与安全；但是，协议的创新就面临了着巨大的困难。导致了Bitcoin Cash社区独立的区块扩容大争论，旷日持久而没有共识的产生，就是一个更加令人不能回避的社会心理学证据。

快速活跃的创新，需要一种无需许可的环境。我们也一直在探索无许可创新的方法，在不需要改变共识的情况下，在Bitcoin Cash的区块链上实现智能合约。经过研究，我们关注到了[OmniLayer协议](https://www.omnilayer.org)，它是一种利用OP_RETURN操作码实现通证发行的方案。这个方案是广受欢迎的泰达币（USDT）日常发行和流通的技术基础。Omni Layer是运行在Bitcoin的区块链之上的。Omni Layer协议采用了[MIT开源许可证](https://github.com/OmniLayer/omnicore/blob/master/COPYING)。我们Fork了Omni Layer的协议，在Bitcoin Cash的区块链上实现了发行通证的技术方案。我们将这种技术方案命名为Wormhole协议，协议中的原生代币命名为Wormhole Cash。

## 安全模型和执行流程
Wormhole协议是通过以bitcoin cash交易作为载体；使用bitcoin cash脚本中特殊的操作码OP_RETURN，将Wormhole协议追加在该操作码后面。通俗来说：Wormhole交易是一种特殊的bitcoin cash交易，与bitcoin cash交易采用相同的安全、确认模型。

[Wormhole](https://github.com/copernet/Wormhole.git) 客户端是在[bitcoin-abc](https://github.com/Bitcoin-ABC/bitcoin-abc.git)客户端上增加功能实现的；是bitcoin-abc的超集，实现了**bitcoin cash**与**Wormhole**两种协议，也是一个**bitcoin cash**的全节点。

Wormhole交易与bitcoin cash 交易的从属关系为：**Wormhole是bitcoin cash的子集**，一个Wormhole交易必定是一个正确的bitcoin cash交易，一个bitcoin cash交易不一定是Wormhole交易。

基于上述前提，只要是正确的**bitcoin cash**交易，Wormhole节点就会接收，并打包验证。

一个Wormhole交易在系统中存在两种状态，有效或失败；不论交易处于哪种状态，它都是一笔正确的bitcoin cash 交易。
*   Wormhole交易的有效状态：满足当前Wormhole交易类型所必需的条件。
    *   示例如下：token转账，发送者账户含有指定token的足够金额。
    *   其他Wormhole的交易类型要求：见[Wormhole-Spec](https://github.com/copernet/spec/blob/master/Wormhole-spec.md)
*   Wormhole交易的无效状态：不能满足当前交易Wormhole交易类型所必需的条件。
    *   示例如下：token转账，转账的token不存在，或发送者账户没有足够的token数量来进行转账。


Wormhole节点的区块处理流程：
*   当节点正常接收区块时(未发生链重组)，节点每接收一个区块后，先对该区块中的所有交易进行`bitcoin cash`协议的检查，不符合`bitcoin cash`协议规则的区块会被丢弃；符合`bitcoin cash`协议规则的区块会进行`Wormhole`协议的检查，在这步，遍历区块中所有的交易，找到符合Wormhole规则的交易并进行验证，更新Wormhole节点的Wormhole数据记录，当区块中的所有交易都处理完毕后，会将当前系统中的Wormhole数据写入磁盘文件，作为当前区块高度的镜像。
*   当节点处于链重组状态时，Wormhole节点会先删除内存中重组区块高度后的所有数据，然后加载分叉点区块的磁盘镜像文件，如果加载成功，则从分叉点重新扫描新链的区块，构建Wormhole数据集，每个区块处理完后，将当前系统的状态写入磁盘，作为当前区块高度的镜像；如果加载镜像文件失败，Wormhole系统删除内存中所有的Wormhole数据集，然后从部署该功能的区块高度开始，重新扫描主链上的所有区块数据，构建Wormhole数据集。

`Wormhole`协议采用的是账户模型，每个`bitcoin cash`地址是一个账户，每个账户地址可以含有多种token，弥补了UTXO模型智能合约的缺失。引用篇文章[Wormhole协议弥补了UTXO模型智能合约的缺失](https://mp.weixin.qq.com/s/dNxkG1yria-C5EPCGPh-Xw)


`Wormhole`的安全有两层保护。

第一层是`Bitcoin Cash`的交易安全，`Bitcoin Cash`采用POW的挖矿算法作为去中心化的时间戳服务器，该算法已经稳定运行将近10年，UTXO模型有以下的一些好处:

*   UTXO无需维护余额
*   UTXO是独立的数据记录单位，可以提升验证交易的速度
*   UTXO模型无需关心事务问题，只关心锁定脚本和解锁脚本
*   UTXO在处理交易的时候具有很高的性能

`Wormhole`协议复用了整个`Bitcoin Cash`中UTXO的安全模型，使用了`Bitcoin Cash`的去中心化时间戳服务器模型。

第二层保护是运行`Wormhole`协议的节点，不符合`Wormhole`协议的数据不会被`Wormhole`协议的节点解析，每个节点都有能力通过重新解析交易数据，计算出`Wormhole`的最近的合法最终状态。

## 未来方向
Wormhole 路线图 : `https://github.com/copernet/spec/blob/master/whcwhitepaper.md#wormhole路线图`

## 结论
智能合约的缺失一直是基于UTXO模型的公链的一大弱点，`Wormhole`协议可以在完全复用UTXO的安全可靠等特性的情况下，增加了账户模型，来实现智能合约，将会给Bitcoin Cash带来更多的可能性。

