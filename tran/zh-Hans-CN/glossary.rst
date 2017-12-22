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

.. _链码:

链码
---------

链码是一个运行在账本上的软件，它可以对资产进行编码，其中的交易
指令（或者叫业务逻辑）也可以用来修改资产。

.. _通道:

通道
-------

A channel is a private blockchain overlay which allows for data
isolation and confidentiality. A channel-specific ledger is shared across the
peers in the channel, and transacting parties must be properly authenticated to
a channel in order to interact with it.  Channels are defined by a
Configuration-Block_.

.. _Commitment:

Commitment
----------

Each Peer_ on a channel validates ordered blocks of
transactions and then commits (writes/appends) the blocks to its replica of the
channel Ledger_. Peers also mark each transaction in each block
as valid or invalid.

.. _Concurrency-Control-Version-Check:

Concurrency Control Version Check
---------------------------------

Concurrency Control Version Check is a method of keeping state in sync across
peers on a channel. Peers execute transactions in parallel, and before commitment
to the ledger, peers check that the data read at execution time has not changed.
If the data read for the transaction has changed between execution time and
commitment time, then a Concurrency Control Version Check violation has
occurred, and the transaction is marked as invalid on the ledger and values
are not updated in the state database.

.. _Configuration-Block:

Configuration Block
-------------------

Contains the configuration data defining members and policies for a system
chain (ordering service) or channel. Any configuration modifications to a
channel or overall network (e.g. a member leaving or joining) will result
in a new configuration block being appended to the appropriate chain. This
block will contain the contents of the genesis block, plus the delta.

.. Consensus

Consensus
---------

A broader term overarching the entire transactional flow, which serves to generate
an agreement on the order and to confirm the correctness of the set of transactions
constituting a block.

.. _Current-State:

Current State
-------------

The current state of the ledger represents the latest values for all keys ever
included in its chain transaction log. Peers commit the latest values to ledger
current state for each valid transaction included in a processed block. Since
current state represents all latest key values known to the channel, it is
sometimes referred to as World State. Chaincode executes transaction proposals
against current state data.

.. _Dynamic-Membership:

Dynamic Membership
------------------

Hyperledger Fabric supports the addition/removal of members, peers, and ordering service
nodes, without compromising the operationality of the overall network. Dynamic
membership is critical when business relationships adjust and entities need to
be added/removed for various reasons.

.. _Endorsement:

Endorsement
-----------

Refers to the process where specific peer nodes execute a chaincode transaction and return
a proposal response to the client application. The proposal response includes the
chaincode execution response message, results (read set and write set), and events,
as well as a signature to serve as proof of the peer's chaincode execution.
Chaincode applications have corresponding endorsement policies, in which the endorsing
peers are specified.

.. _Endorsement-policy:

Endorsement policy
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
