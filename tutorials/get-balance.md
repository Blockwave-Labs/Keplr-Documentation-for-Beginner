# Get Balance

{% hint style="info" %}
If you need a tutorial on Node.js and NPM installation, please check the [top tab](./).
{% endhint %}

Since you have installed all the necessary packages and configured them, let's write a code that performs the function of retrieving Osmo balance from the user's Keplr wallet. First, create a folder called 'functions' under the src folder, create a getBalance.js file under it, and write the code as below.

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



In order to retrieve the osmo balance from the code above, the OSMOSIS chain information is retrieved. The file is as follows. The file location is src/data/chainInfo.js.

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



And let's create a button to do that. Create the components folder under the src folder, create the getBalanceButton.js in it, and write the code below.

```javascript
import React from "react";
import getBalance from "../functions/getBalance";

const GetBalanceButton = () => {
  return <button onClick={getBalance}>Get Balance</button>;
};

export default GetBalanceButton;
```

We're all set. Now, let's modify the App.js that were automatically created when the project was created and write the code so that the corresponding button appears in the browser when the npm start command is executed.

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

After modifying the code as shown above, you can see that the following screen is displayed in the browser by executing the npm start command. If you successfully complete the wallet and network connection by pressing the button, you can see that osmo balance information is output on the developer tool console as follows.

<figure><img src="../.gitbook/assets/Untitled (11).png" alt=""><figcaption></figcaption></figure>
