

## 简介
`Bitcoin Cash`（BCH）在区块高度478,558上产生，一直致力于为世界带来一种可靠的电子现金，履行最初的比特币作为「点对点数字现金」的承诺。其具有全球无缝流通、无许可（Permissionless)创新等特点。在`Bitcoin Cash`如何实现发行通证（Token），众多的开发者已经有过不少的研究，比如染色币的方案[Colored-Coins](https://github.com/Colored-Coins/Colored-Coins-Protocol-Specification/wiki)，之后Andrew Stone 提出了[Enable representative tokens via OP_GROUP on Bitcoin Cash](https://github.com/BitcoinUnlimited/BUIP/blob/master/077.mediawiki)，提议增加OP_GROUP的操作码来实现发Token的方案。OP_GROUP方案需要修改Bitcoin Cash的共识规则才可以实现。更具体地说，类似于在Ethereum网络上广受欢迎的ERC20协议所具备的那些功能。

凡是需要更改共识才能实现的通证发行技术提议，都不可避免地会遇到问题。首先是技术上的风险，其次是对这种风险的顾虑常常引发技术开发社区甚至整个经济生态都陷入巨大的争议。争议中的反对方，其顾虑很可能也确实是真实的。不论这样的争议中谁对谁错，结果常常是有争议的提议无法被实现。这样的困难可以被视为一种保险机制，让具有的风险更改很难被添加到协议之中，保证协议的稳健与安全；但是，协议的创新就面临了着巨大的困难。导致了Bitcoin Cash社区独立的区块扩容大争论，旷日持久而没有共识的产生，就是一个更加令人不能回避的社会心理学证据。

快速活跃的创新，需要一种无需许可的环境。我们也一直在探索无许可创新的方法，在不需要改变共识的情况下，在Bitcoin Cash的区块链上实现智能合约。经过研究，我们关注到了[OmniLayer协议](https://www.omnilayer.org)，它是一种利用OP_RETURN操作码实现通证发行的方案。这个方案是广受欢迎的泰达币（USDT）日常发行和流通的技术基础。Omni Layer是运行在Bitcoin的区块链之上的。Omni Layer协议采用了[MIT开源许可证](https://github.com/OmniLayer/omnicore/blob/master/COPYING)。我们Fork了Omni Layer的协议，在Bitcoin Cash的区块链上实现了发行通证的技术方案。我们将这种技术方案命名为Wormhole协议，协议中的原生代币命名为Wormhole Cash。

## 安全模型和执行流程

## Wormhole 节点

### 节点认知
[Wormhole](https://github.com/copernet/Wormhole.git)节点是[Bitcoin Cash](https://github.com/Bitcoin-ABC/bitcoin-abc.git) 节点的超集，是在`Bitcoin Cash`客户端上增加了Wormhole协议的实现。

`Wormhole`客户端既可以作为一个`Wormhole`全节点使用，同时也可以作为`Bitcoin Cash`全节点使用，它实现了**Bitcoin Cash**协议与**Wormhole**协议。

Wormhole节点接收的交易
*   既可以接收，验证`Wormhole`交易，也可以接收，验证`Bitcoin Cash`交易。


### 节点运行

![wormhole-core.png](https://github.com/copernet/spec/raw/master/image/wormhole-core.png)

Wormhole节点启动后，会与网络中其它节点(包含：Bitcoin Cash节点)通信，接收区块及交易消息；当从网络中收到一个新区块时：进行如下操作
*   先使用`Bitcoin Cash`协议检测该区块及块中的所有交易，符合协议规则，就继续向下执行，否则，丢弃该区块；
*   接着使用`Wormhole`协议检测该区块中的每个交易，使用符合`Wormhole`规则的交易来构建节点中的`Wormhole`数据集；
*   当区块中的所有交易处理完毕后，会对当前节点的`Wormhole`数据集做镜像，并写入磁盘文件。

### Wormhole交易
`Wormhole`交易本质上来说一种特殊的`Bitcoin Cash`交易，利用了`Bitcoin Cash`脚本中一个特殊的操作码`OP_RETURN`，将`Wormhole`协议附加在该操作码后面。

`Wormhole`交易与`Bitcoin Cash`交易具有如下关系
*   `Wormhole`交易是`Bitcoin Cash`交易的子集

`Wormhole`交易在块链中具有两种状态，并且处于这两种状态的交易都是正确的`Bitcoin Cash`交易。
*   有效：`Wormhole`交易满足该交易类型所需的条件；
*   无效：`Wormhole`交易不满足该交易类型所需的条件；
*   `Wormhole`交易类型所需的条件见：[Wormhole-Spec](https://github.com/copernet/spec/blob/master/wormhole-spec.md)

基于上述前提：只要是正确的`Bitcoin Cash`交易，`Wormhole`节点就会接收，并打包验证，因此区块链上会存在状态失败的`Wormhole`交易。

示例：当创建了一笔Wormhole交易，处理流程如下：

![Wormhole-tx运行流程](https://github.com/copernet/spec/raw/master/image/Wormhole-tx.png)

### `Wormhole`账户与`Bitcoin Cash`地址
`Wormhole`协议采用的是账户模型，每个`Bitcoin Cash`地址是一个账户，每个账户可以含有多种类型的Token。

账户模型弥补了`Bitcoin Cash` UTXO模型智能合约的缺失，给`Bitcoin Cash`增加了无限的可能。

引用篇文章[Wormhole协议弥补了UTXO模型智能合约的缺失](https://mp.weixin.qq.com/s/dNxkG1yria-C5EPCGPh-Xw)


### WHC基础货币
`Wormhole`系统创建了一种基础货币`WHC`，为未来智能合约`Gas`收费，链上去中心化交易所，以及创建各种Token提供交易媒介。

系统中`WHC`的来源是通过燃烧`BCH`生成；通过构建一笔燃烧交易，获取相应金额的`WHC`。

燃烧交易的要求如下：
*   第一个交易输出(索引为0)：必须为燃烧输出，即向燃烧地址转`BCH`，金额必须 >= 1 BCH 。
*   第二个交易输出(索引为1)：必须为Wormhole交易的载荷数据，标识该交易为燃烧BCH获取`WHC`类型的交易。
*   注意：
    *   直接向燃烧地址地址转账是不会生成`WHC`，会造成转账的`BCH`丢失；
    *   向燃烧地址转账的`BCH`金额 < 1 BCH 时，也不会生成`WHC`，会造成转账的`BCH`丢失；

兑换比例如下：
*   1 BCH = 100 WHC;   1 WHC = 100,000,000 C ;

燃烧地址：
*   主网：bitcoincash:qqqqqqqqqqqqqqqqqqqqqqqqqqqqqu08dsyxz98whc
*   测试网：bchtest:qqqqqqqqqqqqqqqqqqqqqqqqqqqqqdmwgvnjkt8whc
*   回归测试网：bchreg:pqqqqqqqqqqqqqqqqqqqqqqqqqqqqp0kvc457r8whc

`WHC`的成熟度：在创建燃烧交易后，相应的`WHC`不会立即达到发送者的账户，需要经过一定的确认时间(成熟度)，`WHC`才会到达发送者的账户。

燃烧成熟度：
*   主网：在1000个区块确认后，相应的`WHC`会到达发送者的账户；在随后的区块中，允许对这笔到账的`WHC`进行花费或转账等。
    *   示例：在区块高度500时，确认了1笔有效的燃烧交易(tx1)，此时燃烧成熟度为1；在区块高度1499时，该笔燃烧交易(tx1)被确认了1000次，此时燃烧成熟度为1000，`WHC`到达发送者的账户。在区块高度1499后，允许对这笔燃烧得到的`WHC`进行花费。
*   测试网：在3个区块确认后，相应的`WHC`会到达发送者账户。
*   回归测试网：在1个区块确认后，相应的`WHC`会到达发送者账户。


### Wormhole节点处理区块的流程
在Wormhole的节点中，每个接收的新区块需要被链接到主链，此时会有两种情况出现；
* 将接收的区块追加到当前主链的尾部；
    *   当节点接收新区块后(未发生链重组)，先对该区块中的所有交易进行`Bitcoin Cash`协议的检查，不符合`Bitcoin Cash`协议规则的区块会被丢弃；符合`Bitcoin Cash`协议规则的区块会进行`Wormhole`协议的检查，在这步，遍历区块中所有的交易，找到符合Wormhole规则的交易并进行验证，更新Wormhole节点的Wormhole数据记录，当区块中的所有交易都处理完毕后，会将当前系统中的Wormhole数据写入磁盘文件，作为当前区块高度的镜像。
*  接收新块后，发生块链重组； 
    *   当节点处于链重组状态时，Wormhole节点会先删除内存中重组区块高度后的所有数据，然后加载分叉点区块的磁盘镜像文件，如果加载成功，则从分叉点重新扫描新链的区块，构建Wormhole数据集，每个区块处理完后，将当前系统的状态写入磁盘，作为当前区块高度的镜像；如果加载镜像文件失败，Wormhole系统删除内存中所有的Wormhole数据集，然后从部署该功能的区块高度开始，重新扫描主链上的所有区块数据，构建Wormhole数据集。

### Wormhole的安全模型
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

![Wormhole未来的架构图](https://github.com/copernet/spec/raw/master/image/Wormhole-Architecture.png)

## 结论
智能合约的缺失一直是基于UTXO模型的公链的一大弱点，`Wormhole`协议可以在完全复用UTXO的安全可靠等特性的情况下，增加了账户模型，来实现智能合约，将会给Bitcoin Cash带来更多的可能性。

