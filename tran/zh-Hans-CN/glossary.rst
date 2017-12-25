*需要 Review*

术语表
===========================

专业术语很重要，所有Fabric用户和开发人员
需就每个特定术语的含义达成一致。例如，什么是链码。
该文档其他内容将根据需要引用术语表，当然，
如果你愿意的话，可以随时阅读整个文档；这很有启发性！

.. _Anchor-Peer:

Anchor Peer  锚节点
-----------

锚节点是通道中能被所有其他peer节点探测、并能与之进行通信的一种peer节点。
通道中的每个 成员_ 都有一个（或多个，以防单点故障）锚节点，
允许属于不同成员身份的节点
发现通道中存在的其它节点。


.. _Block:

Block 区块
-----

区块是通道上一组有序交易的集合。区块往往通过密码学手段
连接到前导区块。

.. _Chain:

Chain 链
-----

账本的链是一个交易区块经过“哈希连接”结构化的交易日志。
对等节点从排序服务收到交易区块，
基于背书策略和并发冲突来标注区块的交易为有效或者无效状态，
并且将区块追加到
对等节点文件系统的哈希链中。

.. _chaincode:

Chaincode 链码
---------

链码是一个运行在账本上的软件，它可以对资产进行编码，其中的交易
指令（或者叫业务逻辑）也可以用来修改资产。

.. _Channel:

Channel 通道
-------

通道是私有区块链，它保证了
数据隔离和保密。特定通道的账本由该通道中所有peer节点共享，
并且交易参与方必须经过该通道的正确授权
才能与其交互。通道是由 配置区块_ 
定义的。

.. _Commitment:

Commitment 提交
----------

通道中的每个 peer节点_ 都会验证交易的有序区块，
然后将区块提交（写或追加）至该通道 账本_ 的各个副本。
peer节点也会标记每个区块中的每笔交易的状态
是有效或者无效。

.. _Concurrency-Control-Version-Check:

Concurrency Control Version Check  CCVC
---------------------------------

CCVC是保持通道中各peer节点间状态同步的一种方法。
peer节点并行的执行交易，在交易提交至账本之前，
peer节点会检查已读数据在执行期间是否被修改。
如果读取的数据在执行和提交之间被改变，
就会引发CCVC冲突，
该交易就会在账本中被标记为无效，
而且值不会更新到状态数据库。

.. _Configuration-Block:

Configuration Block 配置区块
-------------------

配置区块包含为系统链（排序服务）或通道定义成员和策略的配置数据。
通道或整个网络的任意配置修改
（比如，成员离开或加入），
都将一个新的配置区块追加到对应的链上。
这个配置区块会包含创世区块的内容和修改后的增量。

.. Consensus

Consensus  共识
---------

共识是贯穿整个交易流程中的广泛意义上的概念，
它会生成对排序的同意
并确认构成区块的一组交易的正确性。

.. _Current-State:

Current State 当前状态
-------------

账本的当前状态代表其链上交易日志中所有Key对应的最新值。
peer节点会将
处理过的区块中的每个有效交易对应修改的最新值提交为账本的当前状态。
由于当前状态表示通道中所有最新的K-V，
所以又被称为 World State。链码执行交易提案就是
针对的当前状态数据。

.. _Dynamic-Membership:

Dynamic Membership 动态成员
------------------

Fabric支持添加/删除成员、peee节点、排序服务节点，
而不会影响整个网络的可操作性。
动态成员至关重要，当业务关系调整并且
由于各种原因需要添加/删除实体时。

.. _Endorsement:

Endorsement 背书
-----------

背书是指一个peer节点执行链码交易并
返回一个提案响应给客户端应用程序的过程。这个提案响应包括
链码执行响应信息、结果（读写集）、事件、
以及作为证明peer节点执行链码的签名。
链码有相应的背书策略，背书策略中指定了
背书节点。

.. _Endorsement-policy:

Endorsement policy 背书策略
------------------

Defines the peer nodes on a channel that must execute transactions attached to a
specific chaincode application, and the required combination of responses (endorsements).
A policy could require that a transaction be endorsed by a minimum number of
endorsing peers, a minimum percentage of endorsing peers, or by all endorsing
peers that are assigned to a specific chaincode application. Policies can be
curated based on the application and the desired level of resilience against
misbehavior (deliberate or not) by the endorsing peers. A transaction that is submitted
must satisfy the endorsement policy before being marked as valid by committing peers.
A distinct endorsement policy for install and instantiate transactions is also required.

.. _Fabric-ca:

Hyperledger Fabric CA
---------------------

Hyperledger Fabric CA is the default Certificate Authority component, which
issues PKI-based certificates to network member organizations and their users.
The CA issues one root certificate (rootCert) to each member and one enrollment
certificate (ECert) to each authorized user.

.. _Genesis-Block:

Genesis Block
-------------

The configuration block that initializes a blockchain network or channel, and
also serves as the first block on a chain.

.. _Gossip-Protocol:

Gossip Protocol
---------------

The gossip data dissemination protocol performs three functions:
1) manages peer discovery and channel membership;
2) disseminates ledger data across all peers on the channel;
3) syncs ledger state across all peers on the channel.
Refer to the :doc:`Gossip <gossip>` topic for more details.

.. _Initialize:

Initialize
----------

A method to initialize a chaincode application.

Install
-------

The process of placing a chaincode on a peer's file system.

Instantiate
-----------

The process of starting and initializing a chaincode application on a specific channel.
After instantiation, peers that have the chaincode installed can accept chaincode
invocations.

.. _Invoke:

Invoke
------

Used to call chaincode functions. A client application invokes chaincode by
sending a transaction proposal to a peer. The peer will execute the chaincode
and return an endorsed proposal response to the client application. The client
application will gather enough proposal responses to satisfy an endorsement policy,
and will then submit the transaction results for ordering, validation, and commit.
The client application may choose not to submit the transaction results. For example
if the invoke only queried the ledger, the client application typically would not
submit the read-only transaction, unless there is desire to log the read on the ledger
for audit purpose. The invoke includes a channel identifier, the chaincode function to
invoke, and an array of arguments.

.. _Leading-Peer:

Leading Peer
------------

Each Member_ can own multiple peers on each channel that
it subscribes to. One of these peers is serves as the leading peer for the channel,
in order to communicate with the network ordering service on behalf of the
member. The ordering service "delivers" blocks to the leading peer(s) on a
channel, who then distribute them to other peers within the same member cluster.

.. _Ledger:

Ledger
------

A ledger is a channel's chain and current state data which is maintained by each
peer on the channel.

.. _Member:

Member
------

A legally separate entity that owns a unique root certificate for the network.
Network components such as peer nodes and application clients will be linked to a member.

.. _MSP:

Membership Service Provider
---------------------------

The Membership Service Provider (MSP) refers to an abstract component of the
system that provides credentials to clients, and peers for them to participate
in a Hyperledger Fabric network. Clients use these credentials to authenticate
their transactions, and peers use these credentials to authenticate transaction
processing results (endorsements). While strongly connected to the transaction
processing components of the systems, this interface aims to have membership
services components defined, in such a way that alternate implementations of
this can be smoothly plugged in without modifying the core of transaction
processing components of the system.

.. _Membership-Services:

Membership Services
-------------------

Membership Services authenticates, authorizes, and manages identities on a
permissioned blockchain network. The membership services code that runs in peers
and orderers both authenticates and authorizes blockchain operations.  It is a
PKI-based implementation of the Membership Services Provider (MSP) abstraction.

.. _Ordering-Service:

Ordering Service
----------------

A defined collective of nodes that orders transactions into a block.  The ordering
service exists independent of the peer processes and orders transactions on a
first-come-first-serve basis for all channel's on the network.  The ordering service is
designed to support pluggable implementations beyond the out-of-the-box SOLO and Kafka varieties.
The ordering service is a common binding for the overall network; it contains the cryptographic
identity material tied to each Member_.

.. _Peer:

Peer
----

A network entity that maintains a ledger and runs chaincode containers in order to perform
read/write operations to the ledger.  Peers are owned and maintained by members.

.. _Policy:

Policy
------

There are policies for endorsement, validation, chaincode
management and network/channel management.

.. _Proposal:

Proposal
--------

A request for endorsement that is aimed at specific peers on a channel. Each
proposal is either an instantiate or an invoke (read/write) request.

.. _Query:

Query 查询
-----

A query is a chaincode invocation which reads the ledger current state but does
not write to the ledger. The chaincode function may query certain keys on the ledger,
or may query for a set of keys on the ledger. Since queries do not change ledger state,
the client application will typically not submit these read-only transactions for ordering,
validation, and commit. Although not typical, the client application can choose to
submit the read-only transaction for ordering, validation, and commit, for example if the
client wants auditable proof on the ledger chain that it had knowledge of specific ledger
state at a certain point in time.

.. _SDK:

Software Development Kit (SDK)
------------------------------

The Hyperledger Fabric client SDK provides a structured environment of libraries
for developers to write and test chaincode applications. The SDK is fully
configurable and extensible through a standard interface. Components, including
cryptographic algorithms for signatures, logging frameworks and state stores,
are easily swapped in and out of the SDK. The SDK provides APIs for transaction
processing, membership services, node traversal and event handling. The SDK
comes in multiple flavors: Node.js, Java. and Python.

.. _State-DB:

State Database 状态数据库
--------------

为了从链码中方便地读写，Current state存储于状态数据库中。
目前支持levelDB和couchDB。

.. _System-Chain:

System Chain 系统链
------------

系统链是包含了在系统层面定义了网络的配置区块的链。
系统链存在于排序服务中，与通道类似，
有初始配置信息，如MSP信息、策略
和配置详情。整个网络的任何变化（如
新组织加入或新排序节点加入）都会导致新的配置区块
添加到系统链。

系统链可以看作是一个或一组通道的通用绑定。
例如，一批金融机构可能组成一个
联盟（由系统链表示），然后根据
各自不同的业务场景创建不同的通道。

.. _Transaction:

Transaction 交易
-----------

交易就是将Invoke或Instantiate结果提交至ordering、validation和commit。
Invoke是从账本中请求读/写数据。Instantiate是
在一个通道中启动和初始化链码的请求。应用程序客户端收集来自背书节点的invoke或Instantiate响应，
并将结果和背书打包
提交至ordering、validation和commit。

.. Licensed under Creative Commons Attribution 4.0 International License
   https://creativecommons.org/licenses/by/4.0/
