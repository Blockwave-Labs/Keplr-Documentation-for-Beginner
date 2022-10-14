# Send Tokens

{% hint style="info" %}
If you need a tutorial on Node.js and NPM installation, please check the [top tab](./).
{% endhint %}

Connect theta-testnet-001 network which is cosmos testnet to my keplr wallet and issue 1 ATOM to my wallet address.



First, run the code from the link below to connect theta-testnet-001 network to my keplr wallet.

{% embed url="https://jsfiddle.net/kht96uvo/1/" %}

Also, you can get a testnet ATOM by putting your cosmos hub wallet address in the link below.

{% embed url="https://www.allthatnode.com/faucet/cosmos.dsrv" %}

Then, you can see that ATOM was created in the keplr wallet cosmos hub testnet as below.

<figure><img src="../.gitbook/assets/Untitled (12).png" alt=""><figcaption></figcaption></figure>

Now let's take this ATOM and create a sendToken function that sends as many ATOMs as you want to other wallet addresses.



Let's create a code similar to the getBalance example above. First, create the sendTokens.js file in the src/functions location and write the code as shown below.

```javascript
import { SigningCosmWasmClient } from "@cosmjs/cosmwasm-stargate";
import { SigningCosmosClient, GasPrice } from "@cosmjs/stargate";
import ChainInfo from "../data/chainInfo_testnet";
import { Decimal } from "@cosmjs/math";

let accounts, CosmWasmClient, queryHandler, gasPrice;

const sendTokens = async () => {
  if (window["keplr"]) {
    if (window.keplr["experimentalSuggestChain"]) {
      await window.keplr.experimentalSuggestChain(ChainInfo);
      await window.keplr.enable(ChainInfo.chainId);
      const offlineSigner = await window.getOfflineSigner(ChainInfo.chainId);
      CosmWasmClient = await SigningCosmWasmClient.connectWithSigner(
        ChainInfo.rpc,
        offlineSigner,
        {
          gasPrice: {
            amount: Decimal.fromAtomics("1000", 4),
            denom: ChainInfo.currencies[0].coinMinimalDenom,
          },
        } // gasfee 설정을 위한 option이 추가됨. 
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

      async function sendTokensTo(address, amount, memo) {
        try {
          let deliverTxResponse = await CosmWasmClient.sendTokens(
            accounts[0].address,
            address,
            [
              {
                denom: ChainInfo.currencies[0].coinMinimalDenom,
                amount: amount,
              },
            ],
            // {
            //   amount: [
            //     {
            //       denom: ChainInfo.currencies[0].coinMinimalDenom,
            //       amount: "1000",
            //     },
            //   ],
            //   gas: "10000", // "1000" = 0.001ATOM
            // },
            "auto", // gasfee 설정에서 "auto"를 사용하려면 위에 처럼 사전에 gas fee option 설정을 해야한다. 그렇지 않으면 자동으로 오류가 발생한다.
            memo
          );
          console.log("Transaction Response", {
            tx: deliverTxResponse,
          });
        } catch (e) {
          console.warn("Error sending tokens", [e, address]);
        }
      }

      var recipientAddress = "cosmos1kxpnf0qls2sn0exh5fhnkgfzg79ffz29e6jytg";
      var amount = "7890";
      var memo = "keplr tutorial test";

      sendTokensTo(recipientAddress, amount, memo);
    } else {
      console.warn(
        "Error accessing experimental features, please update Keplr"
      );
    }
  } else {
    console.warn("Error accessing Keplr, please install Keplr");
  }
};

export default sendTokens;
```

src/functions/sendTokens.js



At this time, the chain you are using is 'theta-testnet-001', which is different from the getbalance example, so create a new chainInfo to fit your network. Create a new chainInfo\_testnet.js under the src/data directory.

```javascript
import { Bech32Address } from "@keplr-wallet/cosmos";

const ChainInfo = {
  chainId: "theta-testnet-001",
  chainName: "theta-testnet-001",
  // rpc: "https://rpc.one.theta-devnet.polypore.xyz",
  // rest: "https://rest.one.theta-devnet.polypore.xyz",
  // rpc: "https://rpc.sentry-01.theta-testnet.polypore.xyz/",
  // rest: "https://rest.sentry-01.theta-testnet.polypore.xyz/",
  rpc: "https://rpc.cosmos.directory/theta",
  rest: "https://rest.cosmos.directory/theta",
  bip44: {
    coinType: 118,
  },
  bech32Config: {
    bech32PrefixAccAddr: "cosmos",
    bech32PrefixAccPub: "cosmos" + "pub",
    bech32PrefixValAddr: "cosmos" + "valoper",
    bech32PrefixValPub: "cosmos" + "valoperpub",
    bech32PrefixConsAddr: "cosmos" + "valcons",
    bech32PrefixConsPub: "cosmos" + "valconspub",
  },
  currencies: [
    {
      coinDenom: "ATOM",
      coinMinimalDenom: "uatom",
      coinDecimals: 6,
      coinGeckoId: "cosmos",
    },
    {
      coinDenom: "THETA",
      coinMinimalDenom: "theta",
      coinDecimals: 0,
    },
    {
      coinDenom: "LAMBDA",
      coinMinimalDenom: "lambda",
      coinDecimals: 0,
    },
    {
      coinDenom: "RHO",
      coinMinimalDenom: "rho",
      coinDecimals: 0,
    },
    {
      coinDenom: "EPSILON",
      coinMinimalDenom: "epsilon",
      coinDecimals: 0,
    },
  ],
  feeCurrencies: [
    {
      coinDenom: "ATOM",
      coinMinimalDenom: "uatom",
      coinDecimals: 6,
      coinGeckoId: "cosmos",
    },
  ],
  stakeCurrency: {
    coinDenom: "ATOM",
    coinMinimalDenom: "uatom",
    coinDecimals: 6,
    coinGeckoId: "cosmos",
  },
  coinType: 118,
  gasPriceStep: {
    low: 1,
    average: 1,
    high: 1,
  },
  features: ["stargate", "ibc-transfer", "no-legacy-stdTx"],
};

export default ChainInfo;
```

src/data/chaininfo\_testnet.js



Then, write the code below again to create a button to perform the above action and add it to App.js.

```javascript
import React from "react";
// import styled from "styled-components";
import sendTokens from "../functions/sendTokens";

const SendTokensButton = () => {
  return <button onClick={sendTokens}>Send Tokens</button>;
};

export default SendTokensButton;
```

src/components/sendTokensButton.js



```javascript
import logo from "./logo.svg";
import "./App.css";
import GetBalanceButton from "./components/getBalanceButton";
import SendTokensButton from "./components/sendTokensButton";

function App() {
  return (
    <div className="App">
      <header className="App-header">
        <GetBalanceButton />
        <SendTokensButton />
      </header>
    </div>
  );
}

export default App;
```

src/App.js



When you press the button to execute the code, a window appears as shown below.

<figure><img src="../.gitbook/assets/Untitled (13).png" alt=""><figcaption></figcaption></figure>

In the window above, click Allow and check the console to see that you return transaction information after successful execution as follows:

<figure><img src="../.gitbook/assets/Untitled (14).png" alt=""><figcaption></figcaption></figure>

```javascript
tx:
	code: 0
	gasUsed: 74136
	gasWanted: 82142
	height: 11910389
	rawLog: "[{\"events\":[{\"type\":\"coin_received\",\"attributes\":[{\"key\":\"receiver\",\"value\":\"cosmos1kxpnf0qls2sn0exh5fhnkgfzg79ffz29e6jytg\"},{\"key\":\"amount\",\"value\":\"7890uatom\"}]},{\"type\":\"coin_spent\",\"attributes\":[{\"key\":\"spender\",\"value\":\"cosmos1kxpnf0qls2sn0exh5fhnkgfzg79ffz29e6jytg\"},{\"key\":\"amount\",\"value\":\"7890uatom\"}]},{\"type\":\"message\",\"attributes\":[{\"key\":\"action\",\"value\":\"/cosmos.bank.v1beta1.MsgSend\"},{\"key\":\"sender\",\"value\":\"cosmos1kxpnf0qls2sn0exh5fhnkgfzg79ffz29e6jytg\"},{\"key\":\"module\",\"value\":\"bank\"}]},{\"type\":\"transfer\",\"attributes\":[{\"key\":\"recipient\",\"value\":\"cosmos1kxpnf0qls2sn0exh5fhnkgfzg79ffz29e6jytg\"},{\"key\":\"sender\",\"value\":\"cosmos1kxpnf0qls2sn0exh5fhnkgfzg79ffz29e6jytg\"},{\"key\":\"amount\",\"value\":\"7890uatom\"}]}]}]"
	transactionHash: "FDD2E314FED186D74EA90782B85F7C2DC1CE7EA225764B0AC593BDB09BC84167"

```

When you checked your wallet, you can confirm that the token transfer was executed properly if only the gas fee you used was left. Since I used 0.082142 Atom as a gas fee, if I check the balance of my wallet, I can see that there is only 0.917858 Atom that 0.082142 Atom escaped from the existing 1 Atom.

<figure><img src="../.gitbook/assets/Untitled (15).png" alt=""><figcaption></figcaption></figure>

In addition, a return from the hash transaction transaction with a value can be seen to have a reference info.



You can then search the corresponding transaction hash value in the cosmos testnet explorer below to view the information in detail.

{% embed url="https://explorer.theta-testnet.polypore.xyz/transactions/FDD2E314FED186D74EA90782B85F7C2DC1CE7EA225764B0AC593BDB09BC84167" %}

<figure><img src="../.gitbook/assets/Untitled (16).png" alt=""><figcaption></figcaption></figure>
