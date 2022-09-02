# Suggest Chain

Keplr's 'suggest chain' feature allows front-ends to request adding new Cosmos-SDK based blockchains that isn't natively integrated to Keplr extension. This allows all Cosmos-SDK blockchains to have permissionless, instant wallet and transaction signing support for front-ends.

프런트엔드는 Keplr의 'Suggest Chain' 기능을 통해 **Keplr 확장에 기본적으로 통합되어 있지 않은 새로운 Cosmos SDK 기반 블록체인 추가를 요청할 수 있습니다**. 이를 통해 모든 Cosmos SDK 블록체인은 프론트 엔드에 대한 무허가, 인스턴트 지갑 및 트랜잭션 서명 지원을 가질 수 있습니다.

If the same chain is already added to Keplr, nothing will happen. If the user rejects the request, an error will be thrown.

이때 **체인은 Cosmos SDK 기반이어야 하며** 동일한 체인이 이미 Keplr에 추가되어 있으면 아무 일도 일어나지 않습니다. 사용자가 요청을 거부하면 오류가 발생합니다.

This feature is often used when connecting testnets. If you'd like to see an example of how to connect to a testnet, please see [here](https://undefined-434.gitbook.io/keplr-documentation/example#juno-testnet). If you want to connect a particular testnet rather than the existing chain provided by the keplr wallet, you can obtain permission through the suggest chain method. If you're curious about the detailed process of connecting the testnet, please refer to it here.

해당 기능은 **테스트넷을 연결할 때에 많이 사용**됩니다. 테스트넷과 연결하는 예시를 보고 싶다면 [이곳](https://undefined-434.gitbook.io/keplr-documentation/example#juno-testnet)을 참조해주세요. keplr 지갑에서 기존에 제공하는 체인이 아닌 특정한 테스트넷을 연결하고자 할 때 suggest chain 메소드를 통해 허가를 받을 수 있습니다. 테스트넷을 연결하는 자세한 과정이 궁금하다면 이곳을 참조해주세요.

Keplr supports the capabilities to contribute to the scalability of the Cosmos ecosystem. Unauthorized transaction signing and other basic account management functions through the Suggest Chain. However, there are differences between native integration and integration through the suggest chain, and some additional features require native integration. Check out the comparison table for the two integrations here.

Keplr는 Cosmos 생태계의 **확장성에 기여하기 위해 해당 기능을 지원하고 있습니다.** Suggest Chain 을 통해 거래 서명 및 기타 기본 계정 관리 기능을 무허가로 제공합니다. 그러나 네이티브 통합과 suggest chain을 통한 통합에는 활용할 수 있는 기능에 대한 차이가 있고 **몇 가지 추가 기능을 구현하려면 네이티브 통합이 필요합니다.** [이곳](https://medium.com/chainapsis/keplr-explained-native-vs-suggest-chain-or-permissionless-integration-8e425f921086)에서 두가지 통합에 대한 비교 표를 확인해보세요.

## Get started with Suggest Chain

```javascript
interface ChainInfo {
    readonly rpc: string;
    readonly rest: string;
    readonly chainId: string;
    readonly chainName: string;
    /**
    * This indicates the type of coin that can be used for stake.
    * You can get actual currency information from Currencies.
    */
    readonly stakeCurrency: Currency;
    readonly walletUrlForStaking?: string;
    readonly bip44: {
        coinType: number;
    };
    readonly alternativeBIP44s?: BIP44[];
    readonly bech32Config: Bech32Config;
    
    readonly currencies: AppCurrency[];
    /**
    * This indicates which coin or token can be used for fee to send transaction.
    * You can get actual currency information from Currencies.
    */
    readonly feeCurrencies: Currency[];
    
    /**
    * This is used to set the fee of the transaction.
    * If this field is empty, it just use the default gas price step (low: 0.01, average: 0.025, high: 0.04).
    * And, set field's type as primitive number because it is hard to restore the prototype after deserialzing if field's type is `Dec`.
    */
    readonly gasPriceStep?: {
        low: number;
        average: number;
        high: number;
    };
    
    /**
    * Indicate the features supported by this chain. Ex) cosmwasm, secretwasm ...
    */
    readonly features?: string[];
}
```

```
experimentalSuggestChain(chainInfo: SuggestingChainInfo): Promise<void>
```

## Usage examples and recommendations

| Key                   | Example Value                                                                                                                                                                                                                                              | Note                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| --------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `rpc`                 | http://123.456.789.012:26657                                                                                                                                                                                                                               | Address of RPC endpoint of the chain. Default port is 26657                                                                                                                                                                                                                                                                                                                                                                                                 |
| `rest`                | http://123.456.789.012:1317                                                                                                                                                                                                                                | Address of REST/API endpoint of the chain. Default port is 1317. Must be enabled in `app.toml`                                                                                                                                                                                                                                                                                                                                                              |
| `chainId`             | mychain-1                                                                                                                                                                                                                                                  | Keplr has a feature which automatically detects when the chain-id has changed, and automatically update to support new chain. However, it should be noted that this functionality will only work when the chain-id follows the {identifier}-{version}(ex.cosmoshub-4) format. Therefore, it is recommended that the chain follows the chain-id format.                                                                                                      |
| `stakeCurrency`       | `{ coinDenom: "ATOM", coinMinimalDenom: "uatom", coinDecimals: 6, coinGeckoId: "cosmos", }`                                                                                                                                                                | Information on the staking token of the chain                                                                                                                                                                                                                                                                                                                                                                                                               |
| `walletUrlForStaking` | [https://wallet.keplr.app/chains/cosmos-hub](https://wallet.keplr.app/chains/cosmos-hub)                                                                                                                                                                   | The URL for the staking interface frontend for the chain. If you don't have a staking interface built, you can use [Lunie Light (opens new window)](https://github.com/luniehq/lunie-light)which supports Keplr.                                                                                                                                                                                                                                            |
| `bip44.coinType`      | 118                                                                                                                                                                                                                                                        | BIP44 coin type for address derivation. We recommend using `118`(Cosmos Hub) as this would provide good Ledger hardware wallet compatibility by utilizing the Cosmos Ledger app.                                                                                                                                                                                                                                                                            |
| `bech32Config`        | `{ bech32PrefixAccAddr: "cosmos", bech32PrefixAccPub: "cosmos" + "pub", bech32PrefixValAddr: "cosmos" + "valoper", bech32PrefixValPub: "cosmos" + "valoperpub", bech32PrefixConsAddr: "cosmos" + "valcons", bech32PrefixConsPub: "cosmos" + "valconspub"}` | Bech32 config using the address prefix of the chain                                                                                                                                                                                                                                                                                                                                                                                                         |
| `currencies`          | `[ { coinDenom: "ATOM", coinMinimalDenom: "uatom", coinDecimals: 6, coinGeckoId: "cosmos", }, ]`                                                                                                                                                           | (TBD)                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| `feeCurrencies`       | `[ { coinDenom: "ATOM", coinMinimalDenom: "uatom", coinDecimals: 6, coinGeckoId: "cosmos", }, ]`                                                                                                                                                           | List of fee tokens accepted by the chain's validator.                                                                                                                                                                                                                                                                                                                                                                                                       |
| `gasPriceStep`        | `{ low: 0.01, average: 0.025, high: 0.03, }`                                                                                                                                                                                                               | Three `gasPrice` values (low, average, high) to estimate transaction fee.                                                                                                                                                                                                                                                                                                                                                                                   |
| `features`            | \[]                                                                                                                                                                                                                                                        | `secretwasm` - Secret Network WASM smart contract transaction support `ibc-transfer` - For IBC transfers (ICS 20) enabled chains. For Stargate (cosmos-sdk v0.40+) chains, Keplr will check the on-chain params and automatically enable IBC transfers if it’s available) `cosmwasm` - For CosmWasm smart contract support (currently broken, in the process of being fixed) `ibc-go` - For chains that use the ibc-go module separated from the cosmos-sdk |



Copy and paste example:

```javascript
await window.keplr.experimentalSuggestChain({
    chainId: "mychain-1",
    chainName: "my new chain",
    rpc: "http://123.456.789.012:26657",
    rest: "http://123.456.789.012:1317",
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
    gasPriceStep: {
        low: 0.01,
        average: 0.025,
        high: 0.03,
    },
});
```

Keplr supports the basic the `x/bank` module's send feature and balance query. Also, it is able to show the staking reward percentage from the `supply` and `mint` module. (For Stargate chains, Keplr will find the supply through the `bank` module).

Keplr는 `x/bank`모듈의 기본 보내기 기능과 잔액 쿼리를 지원합니다. 또한 `supply`및 `mint`모듈에서 스테이킹 보상 비율을 표시할 수 있습니다. (Stargate 체인의 경우, Keplr는 `bank` 모듈을 통해 공급을 찾습니다.
