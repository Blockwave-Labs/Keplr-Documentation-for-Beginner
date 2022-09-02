# Example

## Function by function <a href="#undefined" id="undefined"></a>

### Cosmjs 주요 모듈 별 제공 함수 & 인터페이스

아래 링크에서 쉽게 찾으실 수 있습니다.

* **@cosmjs/cosmwasm-stargate Docs**
* **@cosmjs/stargate**
* **@cosmjs/math**
* **@cosmjs/proto-signing**

### **cosmjs/stargate**

#### Data Type

````javascript
export interface BlockHeader {
  readonly version: {
    readonly block: string;
    readonly app: string;
  };
  readonly height: number;
  readonly chainId: string;
  /** An RFC 3339 time string like e.g. '2020-02-15T10:39:10.4696305Z' */
  readonly time: string;
}

export interface Block {
  /** The ID is a hash of the block header (uppercase hex) */
  readonly id: string;
  readonly header: BlockHeader;
  /** Array of raw transactions */
  readonly txs: readonly Uint8Array[];
}

/** A transaction that is indexed as part of the transaction history */
export interface IndexedTx {
  readonly height: number;
  /** Transaction hash (might be used as transaction ID). Guaranteed to be non-empty upper-case hex */
  readonly hash: string;
  /** Transaction execution error code. 0 on success. */
  readonly code: number;
  readonly rawLog: string;
  /**
   * Raw transaction bytes stored in Tendermint.
   *
   * If you hash this, you get the transaction hash (= transaction ID):
   *
   * ```js
   * import { sha256 } from "@cosmjs/crypto";
   * import { toHex } from "@cosmjs/encoding";
   *
   * const transactionId = toHex(sha256(indexTx.tx)).toUpperCase();
   * ```
   *
   * Use `decodeTxRaw` from @cosmjs/proto-signing to decode this.
   */
  readonly tx: Uint8Array;
  readonly gasUsed: number;
  readonly gasWanted: number;
}

export interface SequenceResponse {
  readonly accountNumber: number;
  readonly sequence: number;
}

/**
 * The response after successfully broadcasting a transaction.
 * Success or failure refer to the execution result.
 */
export interface DeliverTxResponse {
  readonly height: number;
  /** Error code. The transaction suceeded iff code is 0. */
  readonly code: number;
  readonly transactionHash: string;
  readonly rawLog?: string;
  readonly data?: readonly MsgData[];
  readonly gasUsed: number;
  readonly gasWanted: number;
}

export function isDeliverTxFailure(result: DeliverTxResponse): boolean {
  return !!result.code;
}
````

#### Function

| function/method          | parameter                                                                                                   | return                                                                                                                                         | description                                                                                    |
| ------------------------ | ----------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------- |
| isDeliverTxFailure       | `result(DeliverTxResponse)`                                                                                 | `boolean`                                                                                                                                      |                                                                                                |
| isDeliverTxSuccess       | `result(DeliverTxResponse)`                                                                                 | `boolean`                                                                                                                                      |                                                                                                |
| assertIsDeliverTxSuccess | `result(DeliverTxResponse)`                                                                                 | `None`                                                                                                                                         | Throw Error. Ensures the given result is a success. Throws a detailed error message otherwise. |
| assertIsDeliverTxFailure | `result(DeliverTxResponse)`                                                                                 | `None`                                                                                                                                         | Throw Error. Ensures the given result is a success. Throws a detailed error message otherwise. |
| connect                  | <p><code>endpoint(string | HttpEndpoint)</code><br><code>options(StargateClientOptions)</code></p>          | `Promise<StargateClient>`                                                                                                                      |                                                                                                |
| getTmClient              | `None`                                                                                                      | <p><code>Tendermint34Client</code><br><code>undefined</code></p>                                                                               |                                                                                                |
| forceGetTmClient         | `None`                                                                                                      | `Tendermint34Client`                                                                                                                           |                                                                                                |
| getQueryClient           | `None`                                                                                                      | <p><code>QueryClient &#x26; AuthExtension &#x26; BankExtension &#x26; StakingExtension &#x26; TxExtension</code><br><code>undefined</code></p> |                                                                                                |
| forceGetQueryClient      | `None`                                                                                                      | `QueryClient & AuthExtension & BankExtension & StakingExtension & TxExtension`                                                                 |                                                                                                |
| getChainId               | `None`                                                                                                      | `Promise<string>`                                                                                                                              | return chain ID                                                                                |
| getHeight                | `None`                                                                                                      | `Promise<number>`                                                                                                                              |                                                                                                |
| getAccount               | `searchAddress(string)`                                                                                     | `Promise<Account \| null>`                                                                                                                     |                                                                                                |
| getSequence              | `address(string)`                                                                                           | `Promise<SequenceResponse>`                                                                                                                    |                                                                                                |
| getBlock                 | `height(number)`                                                                                            | `Promise<Block>`                                                                                                                               |                                                                                                |
| getBalance               | <p><code>address(string)</code><br><code>searchDenom(string)</code></p>                                     | `Promise<Coin>`                                                                                                                                |                                                                                                |
| getAllBalances           | `address(string)`                                                                                           | `Promise<Coin[]>`                                                                                                                              |                                                                                                |
| getBalanceStaked         | `address(string)`                                                                                           | `Promise<Coin \| null>`                                                                                                                        |                                                                                                |
| getDelegation            | <p><code>delegatorAddress(string)</code><br><code>validatorAddress(string)</code></p>                       | `Promise<Coin \| null>`                                                                                                                        |                                                                                                |
| getTx                    | `id(string)`                                                                                                | `Promise<IndexedTx \| null>`                                                                                                                   |                                                                                                |
| searchTx                 | <p><code>query(SearchTxQuery)</code><br><code>filter(searchTxFilter)</code></p>                             | `Promise<IndexedTx[]>`                                                                                                                         |                                                                                                |
| disconnect               | `None`                                                                                                      | `None`                                                                                                                                         |                                                                                                |
| broadcastTx              | <p><code>tx(Uint8Array)</code><br><code>timeoutMs(number)</code><br><code>pollIntervalMs(number)</code></p> | `Promise<DeliverTxResponse>`                                                                                                                   | timeoutMs’s default value = 60\_000 pollIntervalMs = 3\_000                                    |
| txsQuery                 | `query(string)`                                                                                             | `Promise<IndexedTx[]>`                                                                                                                         |                                                                                                |

```javascript
export function isDeliverTxFailure(result: DeliverTxResponse): boolean {
  return !!result.code;
}

export function isDeliverTxSuccess(result: DeliverTxResponse): boolean {
  return !isDeliverTxFailure(result);
}

/**
 * Ensures the given result is a success. Throws a detailed error message otherwise.
 */
export function assertIsDeliverTxSuccess(result: DeliverTxResponse): void {
  if (isDeliverTxFailure(result)) {
    throw new Error(
      `Error when broadcasting tx ${result.transactionHash} at height ${result.height}. Code: ${result.code}; Raw log: ${result.rawLog}`,
    );
  }
}

/**
 * Ensures the given result is a failure. Throws a detailed error message otherwise.
 */
export function assertIsDeliverTxFailure(result: DeliverTxResponse): void {
  if (isDeliverTxSuccess(result)) {
    throw new Error(
      `Transaction ${result.transactionHash} did not fail at height ${result.height}. Code: ${result.code}; Raw log: ${result.rawLog}`,
    );
  }
}

/** Use for testing only */
export interface PrivateStargateClient {
  readonly tmClient: Tendermint34Client | undefined;
}

export interface StargateClientOptions {
  readonly accountParser?: AccountParser;
}

export class StargateClient {
  private readonly tmClient: Tendermint34Client | undefined;
  private readonly queryClient:
    | (QueryClient & AuthExtension & BankExtension & StakingExtension & TxExtension)
    | undefined;
  private chainId: string | undefined;
  private readonly accountParser: AccountParser;

  public static async connect(
    endpoint: string | HttpEndpoint,
    options: StargateClientOptions = {},
  ): Promise<StargateClient> {
    const tmClient = await Tendermint34Client.connect(endpoint);
    return new StargateClient(tmClient, options);
  }

  protected constructor(tmClient: Tendermint34Client | undefined, options: StargateClientOptions) {
    if (tmClient) {
      this.tmClient = tmClient;
      this.queryClient = QueryClient.withExtensions(
        tmClient,
        setupAuthExtension,
        setupBankExtension,
        setupStakingExtension,
        setupTxExtension,
      );
    }
    const { accountParser = accountFromAny } = options;
    this.accountParser = accountParser;
  }

  protected getTmClient(): Tendermint34Client | undefined {
    return this.tmClient;
  }

  protected forceGetTmClient(): Tendermint34Client {
    if (!this.tmClient) {
      throw new Error(
        "Tendermint client not available. You cannot use online functionality in offline mode.",
      );
    }
    return this.tmClient;
  }

  protected getQueryClient():
    | (QueryClient & AuthExtension & BankExtension & StakingExtension & TxExtension)
    | undefined {
    return this.queryClient;
  }

  protected forceGetQueryClient(): QueryClient &
    AuthExtension &
    BankExtension &
    StakingExtension &
    TxExtension {
    if (!this.queryClient) {
      throw new Error("Query client not available. You cannot use online functionality in offline mode.");
    }
    return this.queryClient;
  }

  public async getChainId(): Promise<string> {
    if (!this.chainId) {
      const response = await this.forceGetTmClient().status();
      const chainId = response.nodeInfo.network;
      if (!chainId) throw new Error("Chain ID must not be empty");
      this.chainId = chainId;
    }

    return this.chainId;
  }

  public async getHeight(): Promise<number> {
    const status = await this.forceGetTmClient().status();
    return status.syncInfo.latestBlockHeight;
  }

  public async getAccount(searchAddress: string): Promise<Account | null> {
    try {
      const account = await this.forceGetQueryClient().auth.account(searchAddress);
      return account ? this.accountParser(account) : null;
    } catch (error: any) {
      if (/rpc error: code = NotFound/i.test(error.toString())) {
        return null;
      }
      throw error;
    }
  }

  public async getSequence(address: string): Promise<SequenceResponse> {
    const account = await this.getAccount(address);
    if (!account) {
      throw new Error(
        "Account does not exist on chain. Send some tokens there before trying to query sequence.",
      );
    }
    return {
      accountNumber: account.accountNumber,
      sequence: account.sequence,
    };
  }

  public async getBlock(height?: number): Promise<Block> {
    const response = await this.forceGetTmClient().block(height);
    return {
      id: toHex(response.blockId.hash).toUpperCase(),
      header: {
        version: {
          block: new Uint53(response.block.header.version.block).toString(),
          app: new Uint53(response.block.header.version.app).toString(),
        },
        height: response.block.header.height,
        chainId: response.block.header.chainId,
        time: toRfc3339WithNanoseconds(response.block.header.time),
      },
      txs: response.block.txs,
    };
  }

  public async getBalance(address: string, searchDenom: string): Promise<Coin> {
    return this.forceGetQueryClient().bank.balance(address, searchDenom);
  }

  /**
   * Queries all balances for all denoms that belong to this address.
   *
   * Uses the grpc queries (which iterates over the store internally), and we cannot get
   * proofs from such a method.
   */
  public async getAllBalances(address: string): Promise<readonly Coin[]> {
    return this.forceGetQueryClient().bank.allBalances(address);
  }

  public async getBalanceStaked(address: string): Promise<Coin | null> {
    const allDelegations = [];
    let startAtKey: Uint8Array | undefined = undefined;
    do {
      const { delegationResponses, pagination }: QueryDelegatorDelegationsResponse =
        await this.forceGetQueryClient().staking.delegatorDelegations(address, startAtKey);

      const loadedDelegations = delegationResponses || [];
      allDelegations.push(...loadedDelegations);
      startAtKey = pagination?.nextKey;
    } while (startAtKey !== undefined && startAtKey.length !== 0);

    const sumValues = allDelegations.reduce(
      (previousValue: Coin | null, currentValue: DelegationResponse): Coin => {
        // Safe because field is set to non-nullable (https://github.com/cosmos/cosmos-sdk/blob/v0.45.3/proto/cosmos/staking/v1beta1/staking.proto#L295)
        assert(currentValue.balance);
        return previousValue !== null ? addCoins(previousValue, currentValue.balance) : currentValue.balance;
      },
      null,
    );

    return sumValues;
  }

  public async getDelegation(delegatorAddress: string, validatorAddress: string): Promise<Coin | null> {
    let delegatedAmount: Coin | undefined;
    try {
      delegatedAmount = (
        await this.forceGetQueryClient().staking.delegation(delegatorAddress, validatorAddress)
      ).delegationResponse?.balance;
    } catch (e: any) {
      if (e.toString().includes("key not found")) {
        // ignore, `delegatedAmount` remains undefined
      } else {
        throw e;
      }
    }
    return delegatedAmount || null;
  }

  public async getTx(id: string): Promise<IndexedTx | null> {
    const results = await this.txsQuery(`tx.hash='${id}'`);
    return results[0] ?? null;
  }

  public async searchTx(query: SearchTxQuery, filter: SearchTxFilter = {}): Promise<readonly IndexedTx[]> {
    const minHeight = filter.minHeight || 0;
    const maxHeight = filter.maxHeight || Number.MAX_SAFE_INTEGER;

    if (maxHeight < minHeight) return []; // optional optimization

    function withFilters(originalQuery: string): string {
      return `${originalQuery} AND tx.height>=${minHeight} AND tx.height<=${maxHeight}`;
    }

    let txs: readonly IndexedTx[];

    if (isSearchByHeightQuery(query)) {
      txs =
        query.height >= minHeight && query.height <= maxHeight
          ? await this.txsQuery(`tx.height=${query.height}`)
          : [];
    } else if (isSearchBySentFromOrToQuery(query)) {
      const sentQuery = withFilters(`message.module='bank' AND transfer.sender='${query.sentFromOrTo}'`);
      const receivedQuery = withFilters(
        `message.module='bank' AND transfer.recipient='${query.sentFromOrTo}'`,
      );
      const [sent, received] = await Promise.all(
        [sentQuery, receivedQuery].map((rawQuery) => this.txsQuery(rawQuery)),
      );
      const sentHashes = sent.map((t) => t.hash);
      txs = [...sent, ...received.filter((t) => !sentHashes.includes(t.hash))];
    } else if (isSearchByTagsQuery(query)) {
      const rawQuery = withFilters(query.tags.map((t) => `${t.key}='${t.value}'`).join(" AND "));
      txs = await this.txsQuery(rawQuery);
    } else {
      throw new Error("Unknown query type");
    }

    const filtered = txs.filter((tx) => tx.height >= minHeight && tx.height <= maxHeight);
    return filtered;
  }

  public disconnect(): void {
    if (this.tmClient) this.tmClient.disconnect();
  }

  /**
   * Broadcasts a signed transaction to the network and monitors its inclusion in a block.
   *
   * If broadcasting is rejected by the node for some reason (e.g. because of a CheckTx failure),
   * an error is thrown.
   *
   * If the transaction is not included in a block before the provided timeout, this errors with a `TimeoutError`.
   *
   * If the transaction is included in a block, a `DeliverTxResponse` is returned. The caller then
   * usually needs to check for execution success or failure.
   */
  public async broadcastTx(
    tx: Uint8Array,
    timeoutMs = 60_000,
    pollIntervalMs = 3_000,
  ): Promise<DeliverTxResponse> {
    let timedOut = false;
    const txPollTimeout = setTimeout(() => {
      timedOut = true;
    }, timeoutMs);

    const pollForTx = async (txId: string): Promise<DeliverTxResponse> => {
      if (timedOut) {
        throw new TimeoutError(
          `Transaction with ID ${txId} was submitted but was not yet found on the chain. You might want to check later. There was a wait of ${
            timeoutMs / 1000
          } seconds.`,
          txId,
        );
      }
      await sleep(pollIntervalMs);
      const result = await this.getTx(txId);
      return result
        ? {
            code: result.code,
            height: result.height,
            rawLog: result.rawLog,
            transactionHash: txId,
            gasUsed: result.gasUsed,
            gasWanted: result.gasWanted,
          }
        : pollForTx(txId);
    };

    const broadcasted = await this.forceGetTmClient().broadcastTxSync({ tx });
    if (broadcasted.code) {
      return Promise.reject(
        new Error(
          `Broadcasting transaction failed with code ${broadcasted.code} (codespace: ${broadcasted.codeSpace}). Log: ${broadcasted.log}`,
        ),
      );
    }
    const transactionId = toHex(broadcasted.hash).toUpperCase();
    return new Promise((resolve, reject) =>
      pollForTx(transactionId).then(
        (value) => {
          clearTimeout(txPollTimeout);
          resolve(value);
        },
        (error) => {
          clearTimeout(txPollTimeout);
          reject(error);
        },
      ),
    );
  }

  private async txsQuery(query: string): Promise<readonly IndexedTx[]> {
    const results = await this.forceGetTmClient().txSearchAll({ query: query });
    return results.txs.map((tx) => {
      return {
        height: tx.height,
        hash: toHex(tx.hash).toUpperCase(),
        code: tx.result.code,
        rawLog: tx.result.log || "",
        tx: tx.tx,
        gasUsed: tx.result.gasUsed,
        gasWanted: tx.result.gasWanted,
      };
    });
  }
}
```

### **cosmjs/cosmwasm-stargate**

| function/method   | parameter                                                                                                                                                                                                                                          | return                          |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------- |
| connectWithSigner | <p><code>endPoint(string | HttpEndpoint)</code><br><code>signer(OfflineSigner)</code><br><code>options(SigningCosmWasmClientOptions)</code></p>                                                                                                    | Promise\<SigningCosmWasmClient> |
| offline           | <p><code>signer(OfflineSigner)</code><br><code>options(SigningCosmWasmClientOptions)</code></p>                                                                                                                                                    | Promise\<SigningCosmWasmClient> |
| constructor       | <p><code>tmClient(Tendermint34Client | undefined)</code><br><code>signer(OfflineSigner)</code><br><code>options(SigningCosmWasmClientOptions)</code></p>                                                                                           | 205                             |
| simulate          | <p><code>signerAddress(string)</code><br><code>messages(readonly EncodeObject[])</code><br><code>memo(string | undefined)</code></p>                                                                                                               | Promise\<number>                |
| upload            | <p><code>senderAddress(string)</code><br><code>wasmCode(Uint8Array)</code><br><code>fee(StdFee | "auto" | number)</code></p>                                                                                                                       | Promise\<UploadResult>          |
| instantiate       | <p><code>senderAddress(string)</code><br><code>codeId(number)</code><br><code>msg(Record&#x3C;string, unknown>)</code><br><code>label(string)</code><br><code>fee(StdFee | "auto" | number)</code><br><code>options(InstantiateOptions)</code></p> | Promise\<InstantiateResult>     |
| updateAdmin       | <p><code>senderAddress(string)</code><br><code>contractAddress(string)</code><br><code>newAdmin(string)</code><br><code>fee(StdFee | "auto" | number)</code></p>                                                                                   | Promise\<ChangeAdminResult>     |
| clearAdmin        | <p><code>senderAddress(string)</code><br><code>contractAddress(string)</code><br><code>fee(StdFee | "auto" | number)</code></p>                                                                                                                    | Promise\<ChangeAdminResult>     |
| migrate           | <p><code>senderAddress(string)</code><br><code>contractAddress(string)</code><br><code>codeId(number)</code><br><code>migrateMsg(Record&#x3C;string, unknown>)</code><br><code>fee(StdFee | "auto" | number)</code></p>                            | Promise\<MigrateResult>         |
| execute           | <p><code>senderAddress(string)</code><br><code>contractAddress(string)</code><br><code>msg(Record&#x3C;string, unknown>)</code><br><code>fee(StdFee | "auto" | number)</code><br><code>funds?(readonly Coin[])</code></p>                          | Promise\<ExecuteResult>         |
| executeMultiple   | <p><code>senderAddress(string)</code><br><code>instructions(readonly ExecuteInstruction[])</code><br><code>fee(StdFee | "auto" | number)</code></p>                                                                                                | Promise\<ExecuteResult>         |
| sendTokens        | <p><code>senderAddress(string)</code><br><code>recipientAddress(string)</code><br><code>amount(readonly Coin[])</code><br><code>fee(StdFee | "auto" | number)</code></p>                                                                           | Promise\<DeliverTxResponse>     |
| delegateTokens    | <p><code>delegatorAddress(string)</code><br><code>validatorAddress(string)</code><br><code>amount(Coin)</code><br><code>fee(StdFee | "auto" | number)</code></p>                                                                                   | Promise\<DeliverTxResponse>     |
| undelegateTokens  | <p><code>delegatorAddress(string)</code><br><code>validatorAddress(string)</code><br><code>amount(Coin)</code><br><code>fee(StdFee | "auto" | number)</code></p>                                                                                   | Promise\<DeliverTxResponse>     |
| withdrawRewards   | <p><code>delegatorAddress(string)</code><br><code>validatorAddress(string)</code><br><code>fee(StdFee | "auto" | number)</code></p>                                                                                                                | Promise\<DeliverTxResponse>     |
| signAndBroadcast  | <p><code>signerAddress(string)</code><br><code>messages(readonly EncodeObject[])</code><br><code>fee(StdFee | "auto" | number)</code></p>                                                                                                          | Promise\<DeliverTxResponse>     |
| sign              | <p><code>signerAddress(string)</code><br><code>messages(readonly EncodeObject[])</code><br><code>fee(StdFee)</code><br><code>memo(string)</code><br><code>explicitSignerData?(SignerData)</code></p>                                               | Promise\<TxRaw>                 |

### cosmjs/encoding

This package is an extension to the JavaScript standard library that is not bound to blockchain products. It provides basic hex/base64/ascii encoding to Uint8Array that doesn't rely on Buffer and also provides better error messages on invalid input.

#### Code Example

```jsx
import { toBech32, toHex, fromHex, fromBech32 } from "@cosmos/encoding";

const example1 =  toBech32("tiov", fromHex("1234ABCD0000AA0000FFFF0000AA00001234ABCD"))
'tiov1zg62hngqqz4qqq8lluqqp2sqqqfrf27dzrrmea';
const example2 = toHex(fromBech32("tiov1zg62hngqqz4qqq8lluqqp2sqqqfrf27dzrrmea").data)
'1234abcd0000aa0000ffff0000aa00001234abcd';
```

#### Why encoding is needed?

여러 주소 표시 체계가 있는데 해당 패키지에서는 Bech32, Hex, Ascii, Base64, RFC3339, UTF8 간의 encoding을 지원합니다.

#### Bech32란?

Bech32 is a [segwit](https://en.bitcoin.it/wiki/Segwit) [address](https://en.bitcoin.it/wiki/Address) format specified by [BIP 0173](https://en.bitcoin.it/wiki/BIP\_0173). This address format is also known as "bc1 addresses". Bech32 is more efficient with block space. As of October 2020, the Bech32 address format is supported in many popular wallets and is the preferred address scheme.

이렇게 블록체제에서 Bech32가 유리하지만 TxHash 등은 Hex이고 상황에 따라 사용해야하는 주소 체계가 있기 때문에 해당 모듈을 사용하면 이를 해결할 수 있습니다.

<제공하는 method 목록>

```jsx
export { fromAscii, toAscii } from "./ascii";
export { fromBase64, toBase64 } from "./base64";
export { Bech32, fromBech32, normalizeBech32, toBech32 } from "./bech32";
export { fromHex, toHex } from "./hex";
export { fromRfc3339, toRfc3339 } from "./rfc3339";
export { fromUtf8, toUtf8 } from "./utf8";
```

모듈의 디테일한 코드, 기능에 대해 알고 싶다면 [이곳](https://github.com/cosmos/cosmjs/tree/main/packages/encoding/src)에서 오픈소스를 참고하길 바랍니다.
