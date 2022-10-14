# Get Contract

{% hint style="info" %}
Node.js와 NPM 설치에 대한 튜토리얼이 필요하다면 [상위 탭](./)을 확인해주세요.
{% endhint %}

```javascript
import { SigningCosmWasmClient } from "@cosmjs/cosmwasm-stargate";
import ChainInfo from "../data/chainInfo";
import { Decimal } from "@cosmjs/math";

let accounts, CosmWasmClient;

const getContracts = async () => {
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

      accounts = await offlineSigner.getAccounts();

      // CosmWasmClient.getSequence(address: string)
      const getSequenceExample = CosmWasmClient.getSequence(
        accounts[0].address
      ).then((sequenceResponse) => {
        console.log("getSequenceResult", sequenceResponse);
      });

			// CosmWasmClient.getHeight()
      const getHeightExample = CosmWasmClient.getHeight().then(
        (heightNumber) => {
          console.log("getHeightResult", heightNumber);

          const getBlockExample = CosmWasmClient.getBlock(heightNumber).then(
            (block) => {
              console.log("getBlockExample", block);
            }
          );
        }
      );

		// CosmWasmClient.getCodes()  
    const getCodesExample = CosmWasmClient.getCodes().then((codeList) => {
        console.log("getCodesResult", codeList);
        var getContractsExample,
          getContractExample,
          getContractCodeHistory,
          queryContractSmartExample;

        codeList?.map(async (code, codeIdx) => {
          console.log("hi");
          getContractsExample = await CosmWasmClient.getContracts(code.id).then(
            (addressList) => {
              console.log(`getContractsResult Id ${code.id}`, addressList);
              addressList?.map(async (addr, addrIdx) => {

								// CosmWasmClient.getContract(address: string) 
								// Throws an error if no contract was found at the address
                getContractExample = await CosmWasmClient.getContract(
                  addr
                ).then((contract) => {
                  console.log(
                    `getContractResult Id ${code.id} Index : ${addrIdx}`,
                    contract
                  );
                });

								// CosmWasmClient.getContractCodeHistory(address: string)  
								// Throws an error if no contract was found at the address
                getContractCodeHistory =
                  await CosmWasmClient.getContractCodeHistory(addr).then(
                    (contractCodeHistoryEntryList) => {
                      console.log(
                        `getContractCodeHistoryResult Id ${code.id} Index : ${addrIdx}`,
                        contractCodeHistoryEntryList
                      );
                    }
                  );
              });
            }
          );
        });
      });
    } else {
      console.warn(
        "Error accessing experimental features, please update Keplr"
      );
    }
  } else {
    console.warn("Error accessing Keplr, please install Keplr");
  }
};

export default getContracts;
```

⚠️ 일부 함수에 대해서는 만약 호출하는 함수에 대한 반환값이 없으면 (내역이 없으면) 오류를 반환하고 Promise가 거부됩니다.
