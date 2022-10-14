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

*   **어플리케이션 레이어**

    블록체인은 분산원장 시스템을 정의할 때 사용하는 일반적인 용어입니다. 이러한 원리로, 각각의 블록체인은 탈중앙화된 어플리케이션을 만들기 위한 기반시설이 됩니다. 예를 들어, 비트코인은 탈중앙화 P2P 결제시스템입니다. 이더리움은 스마트 컨트렉트를 개발하기 위한 블록체인 입니다. 그러므로 각각의 네트워크 공간에서 정의된 프로젝트는 그것만의 비즈니스 논리를 가질 필요가 있습니다. 이것이 다른 프로젝트들을 사용하는 사례를 구분하는 가장 명확한 특징입니다. 어플리케이션 레이어는 각 프로젝트의 구성 로직들이 정의되고, 구현될 수 있는 ‘모듈’이라 할 수 있습니다.

P2P 네트워킹 레이어와 합의 레이어 층을 Tendermint core 라 부릅니다. 이들은 ABCI(Application BlockChain Interface)라는 프로토콜에 의해 애플리케이션 층에 연결됩니다. 물론 Tendermint Core은 블록체인에서의 개발시간을 몇 년에서 몇 주 단위로 단축하지만, 처음부터 ABCI-APP을 구축하는 것은 어렵습니다. 이것이 바로 코스모스 SDK가 존재하는 이유입니다.&#x20;

코스모스 SDK는 텐더민트 코어 위에 안전한 블록체인 에플리케이션 구축 과정을 간소화시키는 일반화된 프레임워크입니다. Cosmos SDK로 개발된 어플리케이션 구조는 프로토콜 연결을 위한 인터페이스 및 서버 상호 작용이 중요합니다. CosmJS(Typescript/Javascript) 라이브러리를 활용하면 개발자가 프론트엔드 사용자 인터페이스와 백엔드 서버 등의 분산 어플리케이션을 구현을 쉽도록 도와주며, 코스모스 생태계와 통합하는 데에 편리하게 사용될 수 있습니다.

</details>
