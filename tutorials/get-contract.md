# Get Contract

{% hint style="info" %}
If you need a tutorial on Node.js and NPM installation, please check the [top tab](./).
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

⚠ For some functions, if there is no return value for the function you call, an error is returned (if there is no history) and Promise is rejected.
