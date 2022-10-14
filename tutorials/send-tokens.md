# Send Tokens

{% hint style="info" %}
If you need a tutorial on Node.js and NPM installation, please check the [top tab](./).
{% endhint %}

나의 keplr 지갑에 cosmos testnet인 theta-testnet-001 네트워크를 연결하고 1 ATOM을 나의 지갑 주소로 발행합니다.



먼저 아래 링크의 코드를 실행시켜 나의 keplr 지갑에 theta-testnet-001 네트워크를 연결합니다.

{% embed url="https://jsfiddle.net/kht96uvo/1/" %}

그리고 아래 링크에서 내 cosmos hub 지갑 주소를 넣어 testnet ATOM을 부여받습니다.

{% embed url="https://www.allthatnode.com/faucet/cosmos.dsrv" %}

그러면 아래와 같이 keplr 지갑 cosmos hub testnet에서 ATOM이 생긴 것을 확인할 수 있습니다.

<figure><img src="../.gitbook/assets/Untitled (12).png" alt=""><figcaption></figcaption></figure>

이제 이 ATOM을 가지고 다른 지갑 주소로 원하는 만큼의 ATOM을 보내는 sendToken 기능을 만들어봅시다.



위의 getBalance 예제와 비슷한 구조로 코드를 짜겠습니다. 먼저 src/functions 위치에 sendTokens.js 파일을 생성하고 아래와 같이 코드를 작성합니다.

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



이 때, 사용하는 체인이 get balance 예제와는 다른 ‘theta-testnet-001’이므로 해당 네트워크에 맞도록 chainInfo를 새로 만들어줍니다. src/data 디렉토리 아래에 새로운 chainInfo\_testnet.js를 만들어줍니다.

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



그리고 위의 동작을 수행할 버튼을 또 다시 아래의 코드를 작성하여 생성해주고 App.js에 추가해줍니다.

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



버튼을 눌러서 코드를 실행시키면 아래와 같은 창이 뜹니다.

<figure><img src="../.gitbook/assets/Untitled (13).png" alt=""><figcaption></figcaption></figure>

위의 창에서 허락을 누르고 콘솔을 확인해보면 다음과 같이 성공적으로 실행 후 transaction 정보를 반환하는 것을 확인할 수 있습니다.

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

내 지갑에서 동일한 내 지갑으로 token을 전송한 것이기 때문에 지갑을 확인했을 때 사용한 gas fee만큼만 빠져나갔다면 제대로 token 전송이 실행됐음을 확인할 수 있습니다. 0.082142 ATOM만큼 gas fee로 사용했기 때문에 지갑의 잔액을 확인해보면 기존의 1 ATOM에서 0.082142 ATOM이 빠져나간 0.917858ATOM 만큼 남아있는 것을 확인할 수 있습니다.

<figure><img src="../.gitbook/assets/Untitled (15).png" alt=""><figcaption></figcaption></figure>

또한 반환 받은 transaction\_hash값을 가지고 transaction info를 조회해볼 수 있습니다.



그러면 아래의 cosmos testnet explorer에서 해당 transaction hash값을 검색하여 정보를 상세히 볼 수 있습니다.

{% embed url="https://explorer.theta-testnet.polypore.xyz/transactions/FDD2E314FED186D74EA90782B85F7C2DC1CE7EA225764B0AC593BDB09BC84167" %}

<figure><img src="../.gitbook/assets/Untitled (16).png" alt=""><figcaption></figcaption></figure>
