---
description: "\bUnderstanding the entire Cosmos ecosystem in easiest way."
---

# Structure

Keplr enables the management of various chains and assets in all Cosmos ecosystems. Among the Cosmos SDK modules, the IBC protocol is a protocol that makes all networks in our ecosystem communicate with each other. Keplr is the only means of supporting the IBC protocol in the Cosmos ecosystem. You can understand the overall structure of the cosmos ecosystem through the toggle below.

Finally, Keplr can easily connect to libraries such as CosmJS, simplifying the process of linking web pages to the blockchain. Click [here](tools-and-libraries/cosmjs.md) to handle Kepler with CosmJS.



<figure><img src=".gitbook/assets/Structure_keplr.jpg" alt=""><figcaption><p>Structure of the Cosmos</p></figcaption></figure>

<details>

<summary>The structure of Cosmos ecosystem</summary>

The Cosmos ecosystem consists of three layers:

* **P2P Networking Layer**

Communication between network participants is an integral part of any blockchain system. The communication methods required for distributed ledger technology-based systems vary widely. Above all, nodes responsible for reaching the consensus layer must communicate with another node to make a decision. Users are able to send transactions through this series of processes. Finally, third-party applications (for example, smart contacts, wallets, or dApps) need to find channels (paths) to trade with the blockchain, and this P2P network layer supports them. In conclusion, the network layer is a layer that is obligated to facilitate and facilitate all kinds of node-to-node communication.

* **Consensus Layer**

Blockchain adopts distributed ledger technology that takes place in the process of validation and solid agreement on orders for transactions. During this process, network participants (miner, verifier, etc.) propose, validate, and handle blocks of transactions. These nodes do not need to trust nodes other than themselves. In addition, the algorithms that perform tasks on different nodes depend on each consensus model. As such, the consensus layer serves as a module that regulates the tasks associated with all consensus processes on the node.

* **Application layer**

Blockchain is a common term used to define distributed ledger systems. With this principle, each blockchain becomes the infrastructure for creating decentralized applications. Bitcoin, for example, is a decentralized P2P payment system. Ethereum is a blockchain for developing smart contacts. Therefore, projects defined in each network space need to have their own business logic. This is the clearest feature that distinguishes cases of using other projects. The application layer is a 'module' in which the configuration logic of each project can be defined and implemented.



The P2P networking layer and the consensus layer are called the Tendermint core. They are connected to the application layer by a protocol called Application Blockchain Interface (ABCI). Of course, Tendermint Core reduces development time on the blockchain from years to weeks, but building ABCI-APP from scratch is difficult. That's why the Cosmos SDK exists.&#x20;

Cosmos SDK is a generalized framework that simplifies the process of building secure blockchain applications on top of a tender mint core. The application architecture developed with Cosmos SDK requires interface and server interaction for protocol connectivity. The CosmJS (Typescript/Javascript) library helps developers easily deploy distributed applications, such as front-end user interfaces and back-end servers, and can be conveniently used to integrate with the Cosmos ecosystem.

</details>
