# Get Balance

{% hint style="info" %}
Node.js와 NPM 설치에 대한 튜토리얼이 필요하다면 [상위 탭](./)을 확인해주세요.
{% endhint %}

필요한 패키지들을 모두 설치하고 환경 설정을 완료하였으므로 사용자의 Keplr지갑에서 Osmo 잔액을 불러오는 기능을 수행하는 코드를 작성해봅시다. 먼저 src 폴더 아래에 functions라는 폴더를 만들고 그 아래에 getBalance.js 파일을 만든 후 아래와 같은 코드를 작성합니다.

```javascript
import { SigningCosmWasmClient } from "@cosmjs/cosmwasm-stargate";
import { SigningCosmosClient, GasPrice } from "@cosmjs/stargate";
import ChainInfo from "../data/chainInfo";

let accounts, CosmWasmClient, queryHandler, gasPrice;

const getBalance = async () => {
  if (window["keplr"]) {
    if (window.keplr["experimentalSuggestChain"]) {
      await window.keplr.experimentalSuggestChain(ChainInfo);
      await window.keplr.enable(ChainInfo.chainId);
      const offlineSigner = await window.getOfflineSigner(ChainInfo.chainId);
      CosmWasmClient = await SigningCosmWasmClient.connectWithSigner(
        ChainInfo.rpc,
        offlineSigner
      );

      // This async waits for the user to authorize your dApp
      // it returns their account address only after permissions
      // to read that address are granted
      accounts = await offlineSigner.getAccounts();

      queryHandler = CosmWasmClient.queryClient.wasm.queryContractSmart;
      // Gas price
      gasPrice = GasPrice.fromString("0.002uconst");

      console.log("Wallet connected", {
        offlineSigner: offlineSigner,
        CosmWasmClient: CosmWasmClient,
        accounts: accounts,
        chain: ChainInfo,
        queryHandler: queryHandler,
        gasPrice: gasPrice,
      });

      async function getAddressBalance(address) {
        try {
          let balance = await CosmWasmClient.getBalance(
            address,
            ChainInfo.currencies[0].coinMinimalDenom
          );
          console.log("Account balance", {
            user: address,
            balance: balance,
          });
          console.log(balance.amount);
        } catch (e) {
          console.warn("Error reading account balance", [e, address]);
        }
      }

      getAddressBalance(accounts[0].address);
    } else {
      console.warn(
        "Error accessing experimental features, please update Keplr"
      );
    }
  } else {
    console.warn("Error accessing Keplr, please install Keplr");
  }
};

export default getBalance;
```

src/functions/getBalance.js



위의 코드에서 osmo 잔액을 불러오기 위해 OSMOSIS chain 정보를 불러오는데 해당 파일은 아래와 같습니다. 파일 위치는 src/data/chainInfo.js입니다.

```javascript
import { Bech32Address } from "@keplr-wallet/cosmos";

const ChainInfo = {
  rpc: "https://rpc-osmosis.blockapsis.com",
  rest: "https://lcd-osmosis.blockapsis.com",
  chainId: "osmosis-1",
  chainName: "Osmosis",
  stakeCurrency: {
    coinDenom: "OSMO",
    coinMinimalDenom: "uosmo",
    coinDecimals: 6,
    coinGeckoId: "osmosis",
    coinImageUrl: window.location.origin + "/public/assets/tokens/osmosis.svg",
  },
  bip44: {
    coinType: 118,
  },
  bech32Config: Bech32Address.defaultBech32Config("osmo"),
  currencies: [
    {
      coinDenom: "OSMO",
      coinMinimalDenom: "uosmo",
      coinDecimals: 6,
      coinGeckoId: "osmosis",
      coinImageUrl:
        window.location.origin + "/public/assets/tokens/osmosis.svg",
    },
    {
      coinDenom: "ION",
      coinMinimalDenom: "uion",
      coinDecimals: 6,
      coinGeckoId: "ion",
      coinImageUrl: window.location.origin + "/public/assets/tokens/ion.png",
    },
  ],
  feeCurrencies: [
    {
      coinDenom: "OSMO",
      coinMinimalDenom: "uosmo",
      coinDecimals: 6,
      coinGeckoId: "osmosis",
      coinImageUrl:
        window.location.origin + "/public/assets/tokens/osmosis.svg",
    },
  ],
  features: ["stargate", "ibc-transfer", "no-legacy-stdTx", "ibc-go"],
  explorerUrlToTx: "https://www.mintscan.io/osmosis/txs/{txHash}",
};

export default ChainInfo;
```

src/data/chaininfo.js



그리고 누르면 해당 작업을 수행하는 버튼을 생성해봅시다. src 폴더 아래에 components 폴더를 만들고 그 안에 getBalanceButton.js를 생성한 후 아래와 같은 코드를 작성합니다.

```javascript
import React from "react";
import getBalance from "../functions/getBalance";

const GetBalanceButton = () => {
  return <button onClick={getBalance}>Get Balance</button>;
};

export default GetBalanceButton;
```

준비가 다 되었습니다. 이제 프로젝트 생성시 자동으로 만들어졌던 App.js를 조금 수정하여 npm start 명령어 실행시 브라우저에서 해당 버튼이 뜨도록 코드를 작성해봅시다.

```javascript
import logo from "./logo.svg";
import "./App.css";
import GetBalanceButton from "./components/getBalanceButton";

function App() {
  return (
    <div className="App">
      <header className="App-header">
        <GetBalanceButton />
      </header>
    </div>
  );
}

export default App;
```

위와 같이 코드를 수정해준 뒤 npm start 명령어를 실행시키면 브라우저에서 다음과 같은 화면이 출력되는 것을 확인할 수 있고 버튼을 눌러 지갑과 네트워크 연결까지 성공적으로 완료하면 개발자 도구 콘솔에 다음과 같이 osmo 잔액 정보가 출력되는 것을 확인할 수 있다.

<figure><img src="../.gitbook/assets/Untitled (11).png" alt=""><figcaption></figcaption></figure>
