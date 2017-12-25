Hyperledger Fabric 是分布式账本技术（DLT）的独特实现，
可确保数据的完整性和一致性，
同时提供其他区块链或DLT技术无法比拟的
审计性、透明度和效率。

Hyperledger Fabric 执行特定类型
:doc:`permissioned <glossary#permissioned-network>` :doc:`blockchain
network <glossary#blockchain-network>` on which members can track,
利用数字化资产交换与互动
:doc:`transactions <glossary#transactions>` 使用智能管理
contracts - what we call :doc:`chaincode <glossary#chaincode>` - in a
secure and robust manner while enabling
:doc:`participants <glossary#participants>` 在网络中进行交互，
在网络互动的方式，可确保他们的交易和数据，
仅限于网络参与者的标识子集，
我们的联系方式a:doc:`channel <glossary#channel>`.

blockchain网络支持成员建立的能力，
共享台账包含真理的来源的数字化，
以安全的方式复制资产和记录的交易，
只对参与该channel的节点集。

Hyperledger Fabric 体系结构由以下组成
组件：对等节点、订购节点和客户端应用程序，
你可以使用 Hyperledger Fabric SDK（例如： java-sdk、nodejs-sdk、python-sdk），
这些组件具有来自证书颁发机构的标识。
Hyperledger Fabric 还提供证书颁发机构服务
*fabric-ca* 但是，你可以用你自己的。

组件：对等节点、订购节点和客户端应用程序，
使用角色对等节点 a :doc:`committer <glossary#commitment>`.
一些人员对等节点也负责通过执行来模拟事务。
chaincodes（智能合同）审批结果。在这个角色中
区块链节点
endorser for certain types of transactions and just a ledger maintainer
(committer) for others.

The :doc:`orderers <glossary#ordering-service>` consent on the order of
transactions in a block to be committed to the ledger. In common
blockchain architectures (including earlier versions of the Hyperledger
Fabric) the roles played by the peer and orderer nodes were unified (cf.
validating peer in Hyperledger Fabric v0.6). The orderers also play a
fundamental role in the creation and management of channels.

Two or more :doc:`participants <glossary#participant>` may create and
join a channel, and begin to interact. Among other things, the policies
governing the channel membership and chaincode lifecycle are specified
at the time of channel creation. Initially, the members in a channel
agree on the terms of the chaincode that will govern the transactions.
When consensus is reached on the :doc:`proposal <glossary#proposal>` to
deploy a given chaincode (as governed by the life cycle policy for the
channel), it is committed to the ledger.

Once the chaincode is deployed to the peer nodes in the channel, :doc:`end
users <glossary#end-users>` with the right privileges can propose
transactions on the channel by using one of the language-specific client
SDKs to invoke functions on the deployed chaincode.

The proposed transactions are sent to endorsers that execute the
chaincode (also called "simulated the transaction"). On successful
execution, endorse the result using the peer's identity and return the
result to the client that initiated the proposal.

The client application ensures that the results from the endorsers are
consistent and signed by the appropriate endorsers, according to the
endorsement policy for that chaincode and, if so, the application then
sends the transaction, comprised of the result and endorsements, to the
ordering service.

Ordering nodes order the transactions - the result and endorsements
received from the clients - into a block which is then sent to the peer
nodes to be committed to the ledger. The peers then validate the
transaction using the endorsement policy for the transaction's chaincode
and against the ledger for consistency of result.

Some key capabilities of Hyperledger Fabric include:

-  Allows for complex query for applications that need ability to handle
   complex data structures.

-  Implements a permissioned network, also known as a consortia network,
   where all members are known to each other.

-  Incorporates a modular approach to various capabilities, enabling
   network designers to plug in their preferred implementations for
   various capabilities such as consensus (ordering), identity
   management, and encryption.

-  Provides a flexible approach for specifying policies and pluggable
   mechanisms to enforce them.

-  Ability to have multiple channels, isolated from one another, that
   allows for multi-lateral transactions amongst select peer nodes,
   thereby ensuring high degrees of privacy and confidentiality required
   by competing businesses and highly regulated industries on a common
   network.

-  Network scalability and performance are achieved through separation
   of chaincode execution from transaction ordering, which limits the
   required levels of trust and verification across nodes for
   optimization.

For a deeper dive into the details, please visit :doc:`this
document <arch-deep-dive>`.

.. Licensed under Creative Commons Attribution 4.0 International License
   https://creativecommons.org/licenses/by/4.0/

