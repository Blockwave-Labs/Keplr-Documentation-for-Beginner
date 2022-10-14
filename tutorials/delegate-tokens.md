# Delegate Tokens

{% hint style="info" %}
Node.js와 NPM 설치에 대한 튜토리얼이 필요하다면 [상위 탭](./)을 확인해주세요.
{% endhint %}

위와 같은 예제들의 구조를 유지하며 다음과 같은 코드를 작성하여 토큰을 위임하는 기능을 구현할 수 있습니다.

```javascript
import { SigningCosmWasmClient } from "@cosmjs/cosmwasm-stargate";
import { SigningCosmosClient, GasPrice } from "@cosmjs/stargate";
import ChainInfo from "../data/chainInfo_testnet";
import { Decimal } from "@cosmjs/math";

let accounts, CosmWasmClient, queryHandler, gasPrice;

const delegateTokens = async () => {
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
        }
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

      async function delegateTokensTo(address, amount, memo) {
        try {
          let deliverTxResponse = await CosmWasmClient.delegateTokens(
            accounts[0].address, //delegatorAddress
            address, //validatorAddress
            [
              {
                denom: ChainInfo.currencies[0].coinMinimalDenom,
                amount: amount,
              },
            ], // amount
            // {
            //   amount: [
            //     {
            //       denom: ChainInfo.currencies[0].coinMinimalDenom,
            //       amount: "1000",
            //     },
            //   ],
            //   gas: "10000", // "1000" = 0.001ATOM
            // },
            "auto", //fee
            memo // memo
          );
          console.log("Transaction Response", {
            tx: deliverTxResponse,
          });
        } catch (e) {
          console.warn("Error sending tokens", [e, address]);
        }
      }

			// validator "cosmostation" address
      var validatorAddress = "osmovaloper1clpqr4nrk4khgkxj78fcwwh6dl3uw4ep88n0y4"; // validator address
      var amount = "7890";
      var memo = "keplr tutorial delegate test";

      delegateTokensTo(validatorAddress, amount, memo);
    } else {
      console.warn(
        "Error accessing experimental features, please update Keplr"
      );
    }
  } else {
    console.warn("Error accessing Keplr, please install Keplr");
  }
};

export default delegateTokens;
```
