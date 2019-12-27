# Polkadot 跨链方案剖析



伴随着 web3.0 的发展，区块链技术也进入到了下一个阶段。为了能打破各个区块链之间的壁垒，更好的拓展区块链的边界，跨链技术得到了大家的重视，也有了较好的发展。PolkaDot 就是其中一个备受期待的跨链解决方案，目前它由前以太坊 CTO Gavin Wood 率领团队开发。那么在互联网科技的新一轮变革悄然开始的背景下，Polkadot 到底是什么？它的出现到底解决了什么问题？以及它又是如何解决的呢？我们会在这篇文章中尝试给出答案。

## Polkadot 概览

### 基本介绍

Polkadot 是一种**异构多链技术，主要由中继链、平行链和转接桥组成**。它的建立是为了连接公有链、联盟链、私有链以及未来可能出现在 web3.0 生态系统中的所有技术。它希望使各个独立的区块链网络都能够通过 Polkadot 的中继链实现信息的交换和无需信任的交易。旨在实现区块链一直在努力实现的 3 个目标：**互操作性**、**可扩展性**、**共享安全性**。

Polkadot 也是一个协议，它允许独立的区块链之间互相交换信息。Polkadot 是一种链间区块链协议（inter-chain blockchain protocol），它与传统互联网的消息传输协议不同（例如 TCP/IP 协议），Polkadot 还会验证各个链之间在进行消息传输时的消息顺序以及消息的有效性。

### 愿景

Polkadot 希望能够连接各个区块链网络，主要关注在解决以下三个层面的问题：

* 互操作性

  当前区块链的生态中，各个区块链网络之间是孤立存在的，它们之间没有任何通信以及互操作的可能性。在未来区块链的世界中也将会存在各种为了满足于某些特定需求的区块链，但若它们仍然彼此孤立将会非常不利于区块链生态的发展。为了能够打破这个壁垒，拓展区块链网络之间的边界，区块链之间的互操作性是一个必须要解决的问题。

  而 Polkadot 的设计目的之一就是让区块链上的 DApp 和智能合约可以无缝地与其他链上的数据或资产进行交易。

* 可拓展性

  在当前大多数区块链中，交易都是在节点中一个一个的处理，因此当交易量逐渐增多的时候，由于网络的限制很容易遇到性能上的瓶颈。

  而 Polkadot 提供了可以运行多个平行链的能力，每个平行链可以并行处理多笔交易，在这种情况下 Polkadot 网络就相当于获得无限的可拓展性。在测试中，Polkadot 网络中的一条平行链每秒大约可以处理 1000 笔交易，通过平行链的创建，就可以成倍的增加每秒交易的数量，从而使 Polkadot 网络具有较高的性能。

![img](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LwLrHqhWhi4vyLfRQAc%2F-LwSYr5tCfB0W9CLeesx%2F-LwSZLMriFjBRc60ZIUi%2FUntitled.png?alt=media&token=b59c437b-bf07-4730-9484-69049c2ca67d)

图片来源于：[https://polkadot.network/Polkadot-lightpaper.pdf](https://polkadot.network/Polkadot-lightpaper.pdf)

* 共享安全性

  基于 PoW 共识的区块链之间存在算力竞争，这样不仅会造成算力等资源的浪费，且一些算力较少的区块链还会非常容易受到攻击，因此各个区块链具有的安全性是不相同的。

  且若各个区块链之间想要互相通信的话，还会由于各个区块链之间的算力不同导致了各个区块链之间不能平等的信任对方，这就比较不利于资产等信息的跨链通信。

  在 Polkadot 网络中，**将由中继链整体负责整个网络的安全**，每个加入 Polkadot 网络中的平行链之间都具有同等程度的安全性，因此它们之间在进行跨链通信时可以充分信任对方的平行链。且**由于 Polkadot 将网络的安全都集中在中继链上，因此，要想攻击整个 Polkadot 网络的难度是非常大的**。

![img](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LwLrHqhWhi4vyLfRQAc%2F-LwSTVsm61NJgQz-Lt11%2F-LwSVd3LNMGMBhxTnCCf%2FUntitled%201.png?alt=media&token=85c44d5f-349a-4a8f-86ac-51a3a172635e)

图片来源于：[https://polkadot.network/Polkadot-lightpaper.pdf](https://polkadot.network/Polkadot-lightpaper.pdf)

### 架构解析

![img](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LwLrHqhWhi4vyLfRQAc%2F-LwSYr5tCfB0W9CLeesx%2F-LwSZ8pVEeC1kJJMdTA8%2FUntitled%202.png?alt=media&token=f06cea3e-7401-4632-a0d2-1c71ab851294)

图 1 \(图片来源于：[https://polkadot.network/PolkaDotPaper.pdf](https://polkadot.network/PolkaDotPaper.pdf)\)

如图 1 所示为 Polkadot 网络的整体架构，从中我们可以看到**中继链（Relay chain）**处于网络的中心位置，它会处理网络中整体的共识和安全性；还有许多**平行链（Parachain）**通过连接中继链以接入 Polkadot 网络中；同时还可以看到在该图的下方有一个**转接桥（bridge）**，这也是 Polkadot 网络中连接了独立区块链（例如：以太坊）的方式。此外还可以看到许多的参与者，例如：收集人（Collator）、验证人（Validator）、钓鱼人（Fisherman）等。那么接下来我们就分别介绍一下在 Polkadot 网络中的主要链角色和不同的参与者。

#### 主要的链角色

* **中继链（Relay Chain）**

  中继链是 Polkadot 网络的中心链，它为整个网络提供了统一的**共识**和**安全性**保障。

  Polkadot 网络中所有的验证人都会在中继链上质押 DOT 代币，从而参与 Polkadot 网络治理。

  由于在 Polkadot 网络中大多数业务相关的操作都会由各个平行链来实现，那么在中继链上就只会发生和网络治理、平行链拍卖等少量的交易类型，因此中继链上的交易手续费通常会高于平行链上的交易手续费。

* **平行链（Parachain）**

  整个 Polkadot 网络中大部分计算工作都将委托给各个平行链进行处理，**平行链会负责具体业务场景的实现**。Polkadot 对平行链的功能**不作任何限制**，平行链可以作为应用链实现任何应用场景，但它们自身却不具备区块的共识能力，它们将共识的职责渡让给了中继链，所有平行链会共享来自中继链的安全保障。平行链之间可以通过 **ICMP**\(Interchain Message Passing\) 进行彼此通信，同时它们还会由分配给它的验证人进行区块验证。

  但平行链有可能不是一条具体的链，Polkadot 中对其定义是：平行链是特定于某些应用程序的数据结构，它在全局上是一致的，并且可以由 Polkadot 中继链的验证人进行验证。一些平行链可能是某些 Dapp 特定的链，也可能是专注于诸如隐私或可拓展性等特定功能的平行链，可能还会存在一些实验性质的平行链，总之平行链在本质上不一定必须是区块链。

![img](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LwLrHqhWhi4vyLfRQAc%2F-LwSZ_g8MKSDaYEnSt1C%2F-LwSZggpUtE4TxAw9OxA%2FUntitled%203.png?alt=media&token=092c6b7e-0f29-4c21-880b-2402fcae664c)

图片来源于：[https://wiki.polkadot.network/docs/assets/network/one\_parachain.png](https://wiki.polkadot.network/docs/assets/network/one_parachain.png)

* **转接桥（Bridge）**

  桥在区块链的链间通信中有着重要作用。在 Polkadot 中关于转接桥的具体实现还有许多待确定的地方，后续官方应该还会有更具体的更新，截止目前在 Polkadot 中转接桥主要由三种不同的含义：

  * **转接桥合约\(Bridge Contracts\)**：通过在 Polkadot 的平行链和外部的区块链（如：Ethereum）上部署智能合约来达到桥的效果，以实现跨链的价值转移。
  * **跨链通信\(Cross-Parachain Communication\)**：由于平行链之间是可以通过 ICMP 进行链间通信的，而无需智能合约承担桥接的功能。且基于 ICMP 的链间通信将是 Polkadot 原生支持的方案。
  * **内置桥接模块\(In-built Bridging Modules\)**：在 Polkadot 网络中，从非平行链中接收平行链上的消息很可能会在 Polkadot 的内置模块中完成。这样的话就不需要在非平行链中部署智能合约扮演“虚拟平行链”了，收集人就可以直接收集整理该区块链上的交易，并提交给中继链了，就像对平行链所做的那样。当前内置的桥接模块可能会考虑基于特定的链进行开发（例如：比特币、以太坊），这就意味着只要是内置交接模块支持的区块链就都可以直接桥接到 Polkadot 网络中了，而无需通过智能合约进行桥接。但对于内置桥接暂不支持的区块链，就还要采用转接桥合约的方法实现了。

#### 主要参与者

![img](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LwLrHqhWhi4vyLfRQAc%2F-LwSZ_g8MKSDaYEnSt1C%2F-LwSZq_MBNpSit9fbHe6%2FUntitled%204.png?alt=media&token=978c6f0e-5fdd-460d-9208-5755549c2860)

图 2 \(图片来源于：[https://polkadot.network/PolkaDotPaper.pdf](https://polkadot.network/PolkaDotPaper.pdf)\)

由图 2 中可知在 Polkadot 网络中，共存在 4 种主要的参与者，分别为**验证人**、**收集人**、**钓鱼人**和**提名人**。简单来看他们的职责是这样的：收集人会从各个平行链中收集交易信息并打包为待验证的区块，接下来一组验证人会对平行链上的区块进行验证；同时网络中的钓鱼人会监控验证人的行为，若钓鱼人发现非法行为就会向其他的验证人进行举报；而提名人会通过向自己信任的验证人质押 token 来参与网络治理。那么他们之间的异同如下分析：

* **验证人（Validator）**

  验证人会负责 Polkadot 网络的出块，它会运行一个中继链节点，在每一轮区块产生中对其提名的平行链出的块进行验证。当平行链出的块都被他们的验证人集合确定好之后，验证人集合会将所有平行链的区块头组装到中继链的区块中并进行共识。

  在 Polkadot 网络中，验证人是以组的形式存在的，它们不是来自于平行链，而是由中继链统一管理的验证人池，通过随机分组指定给平行链的。

* **收集人（Collator）**

  收集人会帮助验证者收集、验证和提交备选的平行链区块，它会维护一个平行链的全节点。

* **钓鱼人（Fisherman）**

  钓鱼人主要靠举报非法交易或者区块以获取收益。在 Polkadot 中钓鱼人是一个软件进程，它会监控 Polkadot 网络中的非法行为，一旦发现就会向 Polkadot 网络举报非法交易。钓鱼人在举报非法交易的时候也需要先质押 token 才行，且举报的交易也要经过共识的过程，只要通过 2/3 及以上其他验证人的验证，就会被打包进区块，若举报的交易是有效的，钓鱼人就会获得相应奖励，若发现钓鱼人进行了虚假的举报，就会没收它质押的 token 作为惩罚。同样对钓鱼人的惩罚和奖励也都是区块链交易。

* **提名人（Nominator）**

  提名人是拥有 token 的相关方，且他希望能通过质押 token 获益。但是它要么是由于 token 份额较少，或者是缺乏维护中继链节点的专业技能，因此他并没有直接作为验证人。

  但是系统还提供了另一种参与网络治理的途径，它不必维护一个中继链节点，但是它可以选择至多 16 个他信任的验证人，并将自己的 stake 通过那个验证人来质押，以此来分享验证人的收益。

#### 链间通信 - ICMP

在 Polkadot 网络的平行链之间，它们通过 ICMP 进行彼此通信。ICMP 意为 Inter-Chain Message Passing，即链间消息传递。以平行链 A 向平行链 B 发送一笔交易为例，简要说明该过程如下：

* 平行链 A 将需跨链的交易放到自己的消息输出队列（egress）中。
* 平行链 A 的收集人在收集交易时会同时拿到这笔跨链交易，并提交给平行链 A 的一组验证人。
* 若平行链 A 的这一组验证人验证成功后，会将本次平行链 A 的区块头信息以及平行链A 中的 egress 队列内的信息提交到中继链上。
* 中继链会运行共识算法进行区块的确认以及**跨链交易路由**，中继链上的验证人会将平行链 A 的相应交易从平行链 A 的 egress 队列中移动到平行链 B 的消息输入队列（ingress）中。
* 平行链 B 会执行该区块，将 ingress 队列中的相应交易执行并修改自身的账本。

以上便是 Polkadot 中基于 ICMP 进行跨链交易的主要步骤。

ICMP 是 Polkadot 网络中的一个协议，它定义了在平行链和/或中继链之间无需额外信任的消息传递的方式。它非常依赖 Polkadot 网络中的中继链存在，若没有了中继链 ICMP 也就无任何意义了。同样，**ICMP 并不是一种消息或者格式的标准**。

就 Polkadot 网络中的共识机制而言，ICMP 协议包含了对消息进行**队列处理、转发**和**排序**的机制。通过和 Polkadot 网络中继链的结合，它可以实现数据的可用性。通过和 Polkadot 网络中平行链的结合，它也实现了消息的输入和输出。但 ICMP 协议并不包含较底层的网络，以及消息语义自身。

总之，ICMP 并不是也不太可是传统意义上的 “标准”，它将会是一个依赖于 Polkadot 的专有技术领域。

#### 共识

在 Polkadot 网络中，同样需要所有的节点达成共识后才能使区块链上的所有状态继续构建并演进，这是一种在共享状态上达成协议的方法。通过共识机制可以使 Polkadot 网络中的节点状态保持彼此同步，它旨在为网络的参与者提供所有状态的客观事实，而针对这个客观事实每个网络参与者又都有自己的主观判断，通过他们之间的沟通并最终达成协议，以此来构建一个新的区块。

**什么是概率确定性和可证明的确定性（Probabilistic vs. provable finality）？**一个运行了 PoW 共识的纯中本聪共识区块链，它只能实现概率上的确定性并最终达到确定性的状态。概率确定性意味着在网络中的某些参与者之下，我们看到了多个区块都连接在了某个区块后，即最长链原则，那么我们就认为它是确定的了。这时最终确定的共识就意味着在将来的某个时刻，网络中所有的节点都将对一组数据的真实性达成共识。而达到这个最终确定的共识可能需要很长时间，并且我们根本无法估算这个时间是多久。但是，诸如 GRANDPA 或以太坊的 Casper FFG 之类的确定性工具，它们旨在为区块的确定性提供更强大、更快速的保证。并且在经过拜占庭协议的某些过程之后，它们将永远无法被还原。这种不可逆共识的概念又被称为可证明的确定性。

在 Polkadot 网络中共有两种不同的共识，分别为 **GRANDPA** 和 **BABE**。之所以会有两种不同的共识是由于 Polkadot 采用了**混合共识**的方式。这里混合共识将区块的产生和区块的最终确定分离开来，其中**BABE 共识用于区块的产生，GRANDPA 共识用于区块的确定**。

混合共识是在 Polkadot 网络中获得概率确定性（产生新区块的能力）和可证明的确定性（在一条链上达成了一次协议，且不可逆）好处的一种方式。通过采用这样的混合共识机制，Polkadot 可以快速生成区块，而速度较慢的确定性机制可以在一个独立的进程中运行以完成区块的确认，而整体不会导致网络交易速度的减慢或阻塞。那么 BABE 和 GRANDPA 到底是什么样的共识机制呢？

![img](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LwLrHqhWhi4vyLfRQAc%2F-LwSZtgd348ld2yHHfI2%2F-LwS__pNWLOkpI-hqQnm%2FUntitled%205.png?alt=media&token=ccd29853-f84f-4ec7-ae15-d48f32e6feea)

图 3 BABE 共识中的出块时间间隔

* **BABE**

  BABE \(**B**lind **A**ssignment for **B**lockchain **E**xtension\) 是一个基于 PoS 的共识协议。它在验证人节点之间运行，并确定由哪个验证人产生新的区块。BABE 和 Ouroboros Praos 算法是比较类似的，只是在 “链的选择规则” 和 “slot 时间” 上有所更改。

  以创世区块为起始，我们将创建区块的后续时间划分为不同的 epoch，再将不同的 epoch 划分为更小时间间隔的 slot，如图 3 所示。**BABE 共识协议中的要点就在于挑选不同的验证人，在每一个 slot 内生成一个区块**。

  BABE 会根据验证人质押的 token 总量，并在每个 epoch 周期内随机为验证人分配 slot 进行区块的生成。在每个 slot 中，Polkadot 中的验证人都会参加一次 “抽奖”，抽奖的结果将会告诉验证人他是否是该 slot 中生成区块的候选人。在 Polkadot 中，一个 slot 有 6 秒钟的时间。由于在每个 epoch 的周期内验证人的选择具有随机性，由此就可能导致一个 slot 中存在多个验证人，也有可能导致某些 slot 中一个验证人都没有，这就可能导致出块时间的不一致，如图 4 所示，有些 slot 中有两个甚至三个验证人，但某些 slot 中却没有验证人。那么要如何处理这两种不同的情况呢。

![img](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LwLrHqhWhi4vyLfRQAc%2F-LwS_np3B30fus0OFKrW%2F-LwS_uBezINnPh7rogAl%2FUntitled%206.png?alt=media&token=e67bfdb2-c4db-4b7d-90f9-327c7594b89b)

图 4（图片来源于：[https://wiki.polkadot.network/docs/assets/VRF\_babe.png）](https://wiki.polkadot.network/docs/assets/VRF_babe.png）)

* slot 有多个验证人

  在一个 slot 中有多个验证人作为生成区块的候选人的时候，每个验证人都会产生一个区块并将其广播到网络中。这时就像是一场比赛一样，谁的块先传播到网络中更多的节点谁就会取得胜利。由于网络的拓扑和网络延迟的影响，可能多条链都会持续向后生成区块，直到最终被确定为止。如图 5 所示，可能会有多条链在同时向后生成区块，而其中最长的链最终可能会被确认。

![img](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LwLrHqhWhi4vyLfRQAc%2F-LwS_np3B30fus0OFKrW%2F-LwSaBkI4giDJQ6fchEx%2FUntitled%207.png?alt=media&token=c9acfc5d-02aa-4d52-ab6e-7d4d287e4871)

图 5（图片来源于：[https://wiki.polkadot.network/docs/en/learn-consensus）](https://wiki.polkadot.network/docs/en/learn-consensus）)

* **slot 中没有验证人**

  当一个 slot 中没有任何验证人在一次抽奖中获胜时，这个 slot 看起来就不能生成区块了。这时 Polkadot 会在后台通过 round-robin 方法选择一个验证人，让他生成一个次级区块（secondary block）。但是一个 slot 中如果已经有了在抽奖中获胜的验证人的话，那么他就会生成一个主要区块（primary block），就不会再有次级区块了。因此，我们会看到每一个 slot 中必定会有一个区块，它要么是主要区块，要么是次级区块。如图 6 所示，在临时分叉的链中可能既有主要区块（图中用 1 表示）也有次级区块（图中用 2 表示）。

![img](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LwLrHqhWhi4vyLfRQAc%2F-LwS_np3B30fus0OFKrW%2F-LwSa1VzviYmQLuDAPid%2FUntitled%208.png?alt=media&token=dd514837-4eab-46a6-a7d1-d856c1e4da92)

图 6（图片来源于：[https://wiki.polkadot.network/docs/en/learn-consensus）](https://wiki.polkadot.network/docs/en/learn-consensus）)

* **GRANDPA**

  GRANDPA \(**G**HOST-based **R**ecursive **AN**cestor **D**eriving **P**refix **A**greement\) 是为 Polkadot 中继链实现的用来最终确定区块的方法。

  只要有 2/3 的节点是诚实的，并且可以在异步的情况下应付 1/5 的拜占庭节点，那么它就可以在部分同步的网络模型中工作。其中有一个显著的区别就是，即使在长期的网络分区中或者其他的网络故障之后，GRANDPA 协议仍然会就某一条链达成共识，而非对某一个区块达成共识，这就极大的加快了区块最终确定的过程。 在 GRANDPA 协议中，所有的节点会为他们所能看到的所有父节点投票，在这种情况下，链上所有的区块就都有了一个票数，如果某个区块所得的票数超过了全网 2/3 的节点，那么就可以最终确定这个区块在最终链上。

  换句话说，只要有超过 2/3 的验证人证明了某条链包含了某个确定的区块，就可以立即最终确定该区块所在的那条链为最终确定的链。

  * 分叉选择

    通过将 BABE 共识和 GRANDPA 共识结合在一起，在面临分叉时 Polkadot 的选择就变得很明确了。BABE 共识会始终在 GRANDPA 最终确定的链上生成区块。在最后确定的区块后面可能会有分叉，BABE 共识会通过在具有最多主要区块（如图 7 中标有 1 的区块）的链上创建区块以提供概率确定性。

    如图 7 所示，其中黑色的区块是已经被最终确定的区块。图中标有 1 的区块代表主要区块，标有 2 的区块代表次级区块。在该图中，即使最上面的一条链是最长链，但是它的主要区块确是最少的，因此它就不符合条件。而第二条链中它的区块总数虽然短，但其主要区块数确是最多的，因此应该选择第二条链。

![img](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LwLrHqhWhi4vyLfRQAc%2F-LwS_np3B30fus0OFKrW%2F-LwSaaTg1a-dRt-F69l5%2FUntitled%209.png?alt=media&token=12573e27-6218-43f0-add5-88d93b939a34)

图 7（图片来源于：[https://wiki.polkadot.network/docs/assets/best\_chain.png）](https://wiki.polkadot.network/docs/assets/best_chain.png）)

### 激励与惩罚

在每个区块链生态中都会有一个经济系统，以使所有的网络参与者能够更好的参与网络的贡献。那么在 Polkadot 中这个经济系统是什么样呢。

#### DOT 代币

DOT 是 Polkadot 网络中的原生代币。DOT 也是持有人在进行投票、验证或委托给其他验证人的许可证明，同样它也可以用来进行交易时支付交易费用。DOT 在 Polkadot 中主要有四个用途：

* **Governance / 治理**

  DOT 的持有者一定程度上决定着 Polkadot 网络的未来。类似的在其他平台中矿工拥有的特权都将给予中继链的参与者，也即 DOT 的持有者。他们可以管理网络中出现的重要事件，例如：协议的升级和修复。

* **Operation / 运营**

  博弈论会激励 DOT 持有者在网络中做好事。没有作恶的参与者将会通过这种机制获得相应的回报，而在网络中作恶的参与者就会失去他们的 DOT。这样也可以一定程度上保证网络的安全。

* **Inter-operability / 互操作性**

  对于要从一条区块链发送到下一条区块链的消息，发送者可以支付一定的 DOT 作为交易费用，但这并不是必须的。

* **Bonding / 质押**

  新的平行链需要通过绑定（bond）DOT 来加入网络中。且无人维护的和无用的平行链也可以通过解除绑定 DOT 以从网络中删除。这也是 PoS 的一种形式。

DOT token 将会在主网上线之后才能使用。

#### 平行链的插槽拍卖

根据目前的估算，平行链的数量上限为 100 条，不过未来有可能减少或增加。

Polkadot 网络通过拍卖机制来竞拍平行链的使用权 —— 出价最高的人需要在 PoS 系统中锁定一定数量的 DOT，才可以在一定时间段内拥有所拍得平行链的使用权。

这意味着要想使用 Polkadot 上的平行链，需要购买并锁定大量 DOT ，直到不想再使用这条平行链为止。

#### 参与者激励和惩罚

* 验证人激励

  验证人通过质押 token 在中继链上进行区块的确认，当区块被确认时，它会收到对应的奖励。如果作恶，被钓鱼人举报并且验证为真实作恶的话，就会对该验证人进行惩罚。

* 收集人激励

  收集人会收集平行链中的交易，这些交易中的手续费会作为收集人的奖励。

* 钓鱼人激励

  钓鱼人对 Polkadot 网络中的验证人进行监控，若发现非法的交易，它就会通过质押 token 来举报给其他组的验证人进行验证，若其他验证人证明了举报是真实的，钓鱼人就会获得相应的奖励，反之，若为虚假举报，钓鱼人就会受到相应惩罚，他所质押的 token 也会被没收。

* 提名人激励

  提名人最多可以支持 16 个他们信任的验证人候选人，并将自己的 token 通过自己所信任的验证人进行质押，这样就可以和验证人共享其收益。

  在 Polkadot 的质押机制中，所有验证人将会获得相同的奖励，而所有参与质押的提名人将会等验证人减去所设置的佣金后，再按他们的抵押比例分配奖励。

### 生态

#### 概述

如图 8 所示是 [PolkaProject](http://polkaproject.com/) 发布的关于 Polkadot 生态中的所有项目，可以看到 Polkadot 生态中的项目还是蛮多的，其中钱包、区块链浏览器、基础设施项目，还有一些论坛等。其中的优秀项目还是比较多的，比如：ChainX、Edgeware、Darwinia、Cdot 等。稍候我们会对这些优秀的项目做一些简要的介绍。在这之前，我们先来看下 Polkadot 的开发者生态如何。

![img](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LwLrHqhWhi4vyLfRQAc%2F-LwSeUZi2p5A7EAFpfqg%2F-LwSfDbLwEJMBP2Gs2UA%2Fimage.png?alt=media&token=1133e1fd-7ad7-445f-9cbc-2ec31e488918)

图 8（图片来源于：[https://pbs.twimg.com/media/EJojsU7U4AAE5Ag?format=jpg&name=large）](https://pbs.twimg.com/media/EJojsU7U4AAE5Ag?format=jpg&name=large）)

从 [Polkadot](https://github.com/paritytech/polkadot) 在 GitHub 中的开源项目来看，几乎每天都有代码相关的更新，且大多数更新都是 Gavin 的 commit。而同样由其开发的区块链开发框架 [Substrate](https://github.com/paritytech/substrate) 更是每天都有足量的代码更新。如下面两张图所示，我们有理由相信 Polkadot 生态中的核心项目目前均是处于正常的维护中的。且在 Riot 聊天软件中，有「Substrate Technical」和「Polkadot Watercooler」两个聊天室，且在 Telegram 中也有「Polkadot Network \(Official\)」聊天室，该聊天室中差不多有五千人，每天都会有不少小伙伴在里面提问技术问题和 Polkadot 相关的问题，且都能得到及时的回应。因此， Polkadot 还是有一个比较活跃的开发者生态的。

![img](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LwLrHqhWhi4vyLfRQAc%2F-LwSbJ3oFaEyEcqc_s1O%2F-LwSbUcr73R90BZUNc-h%2FUntitled%2011.png?alt=media&token=0f55dc67-ccfe-433a-bfbe-d0ff4a4d3b4d)

![img](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LwLrHqhWhi4vyLfRQAc%2F-LwSbJ3oFaEyEcqc_s1O%2F-LwSbdn0-nYyK-NxGQFZ%2FUntitled%2012.png?alt=media&token=cccc0de0-578e-4a1b-847b-98dbd37b2e3d)

#### [Substrate](https://substrate.dev/)

提到 Polkadot 生态，就不得不提 Substrate 了。Substrate 是 Parity 公司在开发 Polkadot 过程中的抽象出来的区块链开发框架。他们在开发 Polkadot 的过程中慢慢发现，有许多机制可能是每一个区块链都应该具有的，比如：共识、质押、账户等等，他们就逐渐将这些模块抽象出来，直到最终抽象出了 Substrate 这个下一代区块链开发框架。

Substrate 由 Rust 语言开发，我们可以通过 Substrate 框架创建一条完整的、可配置的区块链。通过 Substrate，我们可以获得许多开箱即用的功能，同样还可以添加自定义功能：

* 一条基于 PoS 的区块链
* 可升级的运行时环境
* 可插拔的共识 \(PoS, PoW, PoA\)
* P2P 网络层
* 内置的基本加密工具
* 支持轻客户端

目前，Substrate 已经是独立于 Polkadot 的项目了，现在由单独的团队进行开发维护。尽管 Polkadot 是一个基于 Substrate 构建的项目，且任何基于 Substrate 其他的项目都接入 Polkadot 网络运行。我们现在就可以使用 Substrate 构建一条新的区块链，而不用等待 Polkadot 开发完成，就可以开始使用此框架开发区块链了。

#### [ChainX](https://chainx.org/)

ChainX 是 Polkadot 生态中的一款明星项目，它的愿景是打破链间资产壁垒，以实现多币种融合的公链生态。ChainX 的系统架构中主要包含三个步骤：

* ChainX 1.0 主要实现一个单链系统，基于区块链框架 Substrate 实现，目的是为了成为 Polkadot 的一条平行链。且 ChainX v1 已经早于 Polkadot 在 2019 年 5 ⽉上线，它现在已经作为独⽴链运⾏并发⾏了 ChainX 原生 Token 名为 PCX。但由于 Polkadot 的开发延时，Polkadot 尚未主网上线，因此 ChainX 现在仍然处于单链运行状态，无法作为平行链接入 Polkadot 中。
* ChainX 2.0 主要为了实现一个双链系统，即新增⼀条转接桥链作为 Polkadot 的平⾏链，现在 ChainX 2.0 开发过的功能就包含 BTC 转接桥，以及支持 BTC 的 WASM 智能合约平台。ChainX v2 预计于 Polkadot 发布 v1 后再上线。
* ChainX 3.0 主要是想实现一个多链系统，在 3.0 阶段，ChainX 将拆分为多链架构，作为 Polkadot 的第⼆层中继⽹络运⾏。其中 Polkadot 专注于底层消息跨链，而 ChainX 专注于实现其内部的资产跨链。如下图所示。其中：
  * ChainX 中继链：是 ChainX 全系统的最⾼安全性保障，负责第⼆层⽹络的整体共享安全共识。
  * 转接桥平行链：用于将各个转接桥拆分为独立的平行链，用于分担压力。
  * 交易平行链：为全系统的资产提供免费撮合服务，提升交易吞吐量。
  * DAPP 平行链：社区开发的各类应用可以独立运行，并保持跨链通信能力。

![img](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LwLrHqhWhi4vyLfRQAc%2F-LwS_np3B30fus0OFKrW%2F-LwSaqvYoFfQgXrj0ZRN%2FUntitled%2013.png?alt=media&token=20bf2ff2-7649-42f7-986d-b6036b733de7)

图片来源于：[https://chainx.org/static/1326c0c531c7770e90eb72ef364585ca/f4193/section3\_3.png](https://chainx.org/static/1326c0c531c7770e90eb72ef364585ca/f4193/section3_3.png)

#### [Darwinia Network](https://darwinia.network/)

Darwinia（达尔文网络）在其官网中说自己是为面向用户的区块链应用开发者打造的 Polkadot 平行链。**在业务场景中，达尔文网络主要专注于游戏区块链的跨链问题**。

达尔文网络希望使用区块链技术和框架来构造一个开放的网络和应用开发套件。这个网络和开发套件将应用于区块链可信技术和 Web3 的基础设施，同时又能够具备以下特征，即：分层的网络设计、支持跨链交互、对开发者友好、拥有最佳的用户体验，且高度可定制。在这种愿景的驱使下，他们希望能够打造达尔文网络和达尔文应用链 SDK。

达尔⽂网络是基于 Substrate 框架构建的区块链网络，它在架构的设计上参考了 Polkadot 的跨链⽹络框架，其中包括中继链，平行链，转接桥等设计。达尔文网络是 Polkadot ⽣态中的⼀员，但同时又区分于 Polkadot 的是，达尔文网络主要专注于游戏和应用方向的跨链和应⽤链业务上。它的技术架构图如下图所示。而达尔文中继链会在将来 Polkadot 主网上线时作为平行链接入 Polkadot 网络中。

![img](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LwLrHqhWhi4vyLfRQAc%2F-LwSd469FydA2lUsuLHB%2F-LwSdqFRt4fwyfh1PlRj%2Fimage.png?alt=media&token=a0c52e82-dffd-44c4-83bb-1594c4252ab4)

图片来源于：[https://darwinia.network/static/media/architecture.389ccdfd.png](https://darwinia.network/static/media/architecture.389ccdfd.png)

达尔文网络目前处于测试网阶段，它的整体发展路线图如下。可以看到达尔文网络预计 2019 年 12 月主网上线，到 2020 年 2 月发布其应用链 SDK。

![img](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LwLrHqhWhi4vyLfRQAc%2F-LwS_np3B30fus0OFKrW%2F-LwSb8DHSNKoa9KHjsoi%2FUntitled%2015.png?alt=media&token=014e7fd1-5866-4ed5-bbb1-c15aefc47c73)

图片来源于：[https://darwinia.network/](https://darwinia.network/)

#### [Edgeware](https://edgewa.re/)

Edgeware 是 Polkadot 生态中第一个基于 WASM 的智能合约平台，它也是 Polkadot 网络中的一条平行链，拥有 Polkadot 网络的拓展性和安全性。

Edgeware 基于 Substrate 框架开发。由于 Substrate 实现了启动工作区块链所需的几乎所有代码，包括 libp2p 网络、WebAssembly 运行、PBFT 共识以及运行验证节点的客户端。因此，Edgeware 的工作主要包含区块链治理系统的构建，用于编写可编译为以太坊 WebAssembly 的 C / C ++ / Rust 智能合约，并移植现有 EVM 智能合约，最终在 Ewasm 上运行。

#### [Cdot Network](https://cdot.network/)

Cdot Network 希望打造 Polkadot 生态中的跨链枢纽，即 Cdot Hub，用以将 Polkadot 中的平行链和一些主流的公链（也可以包括比较重要的一些联盟链等）连接起来。

Cdot Hub 基于 Substrate 框架开发，采用 Tendermint 共识，并将实现 IBC 跨链协议，这使得应用链只要低成本接入 Cdot 就可以获得跨链互操作能力。我们可以看到 Cdot 希望能够实现 IBC 协议，因此，相信将来 Cdot 上线后是有希望成为连接 Cosmos 生态和 Polkadot 生态的桥梁项目的。

## 评估

Polkadot 作为一个跨链的技术解决方案，它同样会面临跨链本身所需要解决的问题，包含跨链交易的**原子性**、**可验证性**、**数据一致性**以及**安全性**问题。在 Polkadot 中，主要通过中继链、平行链以及 ICMP 机制解决这些问题。Polkadot 网络中所有的平行链会通过中继链共享网络整体的安全，通过 ICMP 协议由验证人实现不同消息在不同平行链间的转移。且在 Polkadot 网络中还有相应的激励和惩罚措施，以提高验证人作恶的成本，保证网络的安全。

以上我们分别介绍了 Polkadot 网络的基本结构，包含所有的链角色和网络中参与者的角色，以及它的共识机制和激励惩罚措施等，那么总体上来看 Polkadot 怎么样，又有什么优劣势呢。

### 优势

* Polkadot 网络由中继链统一管理网络的共识，所有平行链**共享安全**，且所有平行链都具有同等级别的安全，在共享安全性模型下的跨链通信更容易解决数据可用性问题。
* 由于 Polkadot 中的共享安全性，应用链（平行链）的开发就不需要自己维护区块链的安全，能够**降低区块链的开发成本**。
* Polkadot 将**区块生成的共识和区块确认的共识分离开来**，采用 BABE 和 GRANDPA 的混合共识模式，能够加快网络中的**共识效率**。
* 开发者可基于 Substrate 区块链开发框架构建一条应用链，该区块链将原生支持 Polkadot  生态内的跨链功能，且其中包括各种**开箱即用**的模块，例如治理模块（投票系统）、质押模块和认证模块等，可以用来**快速搭建一条区块链，并接入 Polkadot 网络**。
* 在 Polkadot 网络中，由中继链维护着网络整体的 world state，平行链之间通过 ICMP 实现跨链通信。且平行链之间不仅仅可以进行跨链转账，还可以进行**任意信息的通信**。这就意味着一个平行链可以对另外一个平行链发起智能合约调用（call smart contract）。但还未实现。

### 劣势

* **平行链的加入成本较高。**根据 web3 foundation \(Polkadot 背后的基金会，它们会对 Polkadot 生态做技术研究和营销\) 对平行链租赁的解释来看，平行链上的资源是有限的，所以一开始的平行链会以拍卖的形式做，后续会以租赁的形式继续维护。而要想拍卖或者租赁平行链是需要用 DOT 进行质押的，这样就可能导致平行链的维护成本太高，只有拥有大量 DOT 的链才能进入 Polkadot 网络中。
* 同样是由于 Polkadot 网络的共享安全性，导致了整个网络具有一定的 “连坐性”，若 Polkadot 网络中继链遭受攻击，就会导致整个网络受到伤害。
* ICMP 协议还未实现，且截至目前主网还未上线，因此许多想法还未得到验证。

## 总结

综上，**Polkadot 是一个创新型的跨链项目，提出了中继链、平行链等概念，并将区块的产生和确定共识区分开来，同时发布了 Substrate 框架用来构建区块链平台，以及通过 ICMP 来实现平行链间的跨链通信**。通过我们在这段时间对 Polkadot 的学习和研究，我们发现对于跨链机制 ICMP 来说，它还处在一个较为前期的阶段，最近在社区中刚刚得知，官方接下来才准备发布关于 ICMP 相关的 paper，从实现上来看也是同样还处于暂未完成的状态。但同时我们也看到有许多项目在向 Substrate 框架迁移，比如：[Shift](https://www.shiftproject.com/)。在 GitHub 中可以看到 Substrate 的代码库每天都有巨量的代码更新，且 Substrate 还拥有较为活跃的开发者社区。因此，我们相信 Substrate 是一个具有前景的区块链开发框架。对于 Polkadot 生态中跨链方案 ICMP 前景的看法，我们目前是比较保守的，但我们会持续关注它的发展动态。

互联互通是大势所趋，任何一个生态如果选择孤立发展，就会被区块链互联网产生巨大的网络效应所挤压，最终遭受淘汰。总体上，Polkadot 大大降低了应用链的开发成本，共识等模块都不需要自己实现，可以直接依赖中继链，且只要平行链在 Polkadot 网络中上线就可以和所有的平行链享受平等级别的安全等级，看起来这会是一个很吸引人的点。但 Polkadot 中有限的平行链资源以及拍卖入场的方式，又未免会让 DOT 持有量少的参与者望而却步，这就加大了 Polkadot 网络的入场门槛。总之，我们应当保持一个开放的心态，在跨链解决方案百家争鸣的时代什么都有可能发生，我们更应该对技术本身进行深入了解，了解其优劣，架构方式等。

## Glossary

* **ICMP**：Inter-chain Message Passage，是 Polkadot 中用于解决跨链通信的协议。
* **中继链**：Polkadot 网络中的一条链，主要负责 Polkadot 网络的共识和平行链之间的通信。
* **平行链**：在 Polkadot 网络中运行的一条区块链，它实现了某些具体的应用场景，但不具备共识能力，是一条应用链。
* **转接桥**：在 Polkadot 网络中充当中继链和外部区块链（例如：以太坊）之间的中介，通过转接桥，可以使外部区块链作为一个 “虚拟平行链” 接入中继链，以使外部区块链和 Polkadot 网络中的链进行网络通信。
* **Collator**：它是一个节点，通过收集平行链上的交易并为验证者生成状态转换证明（state transition proofs）来维护平行链的节点。
* **Validator**：它是一个节点，通过质押 DOT 来验证平行链上的来自于收集人的状态转换证明并与其他验证人一起确定共识来维护中继链。
* **钓鱼人\(Fisherman\)**：它们负责监视网络中的验证人和收集人，只要他们作恶，钓鱼人就可以通过质押少量的 DOT 来进行举报，但是如果他们真的发现了作恶行为，就可以得到大量的收益回报。
* **Epoch**：epoch 是 BABE 协议中的一段用于出块的时间，它又被划分为不同的 slot，每个 slot 是一个更小的时间间隔，在 Polkadot 中默认为 6 秒，且在一个 slot 中会有一个区块的产生。
* **Ewasm**：WebAssembly for Ethereum，是以太坊下一代虚拟机。
* **绑定（Bonding）**：是一个将 token “冻结” 的过程，通过 “冻结” token，才可以使平行链接入到中继链中。

## 参考文献

1. [Polkadot · Polkadot Wiki](https://wiki.polkadot.network/docs/en/learn-introduction)
2. [Parachains · Polkadot Wiki](https://wiki.polkadot.network/docs/en/learn-parachains)
3. [Bridges · Polkadot Wiki](https://wiki.polkadot.network/docs/en/learn-bridges)
4. [https://polkadot.network/Polkadot-lightpaper.pdf](https://polkadot.network/Polkadot-lightpaper.pdf)
5. [https://github.com/paritytech/polkadot/wiki/ICMP](https://github.com/paritytech/polkadot/wiki/ICMP)
6. [Polkadot Consensus · Polkadot Wiki](https://wiki.polkadot.network/docs/en/learn-consensus)
7. [Polkadot Consensus · Polkadot Wiki](https://wiki.polkadot.network/docs/en/learn-consensus)
8. [Polkadot Network](https://polkadot.network/dot-token/)
9. [抵押 · Polkadot Wiki](https://wiki.polkadot.network/docs/zh-CN/learn-staking)
10. [跨链天王 Cosmos、Polkadot 的深层区别](https://www.chainnews.com/articles/976923251427.htm)
11. [ChainX: Crypto Asset Gateway for Polkadot Ecosystem](https://chainx.org/)
12. [Darwinia Network - 达尔文网络](https://darwinia.network/)

