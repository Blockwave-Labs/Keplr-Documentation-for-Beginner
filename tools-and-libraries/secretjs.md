# SecretJs

If you want to connect Keplr to a secret network within the Cosmos ecosystem, you can easily connect using SecretJs. SecretJs is a JavaScript SDK for writing applications that interact with the Secret Network blockchain. If you are curious about the Secret Network in the Cosmos ecosystem, you can check it out [here.](https://scrt.network/about/about-secret-network/)

Written in TypeScript and provided with type definitions.

* Provides simple abstractions over core data structures.
* Supports every possible message and transaction type.
* Exposes every possible query type.
* Handles input/output encryption/decryption for Secret Contracts.
* Works in Node.js, modern web browsers and React Native.

## Get started with SecretJs

### Connecting with SecretJs

SecretJS link: [https://www.npmjs.com/package/secretjs (opens new window)](https://www.npmjs.com/package/secretjs)The basics of using SecretJS is similar to CosmJS. Refer to the [Use with CosmJs](cosmjs.md) section for more information.

One difference between CosmJS and SecretJS is that we recommend using Keplr's `EnigmaUtils`. By using Keplr's `EnigmaUtils`, you can use Keplr to encrypt/decrypt, and the decrypted transaction messages are shown to the user in a human-readable format.

```javascript
// Enabling before using the Keplr is recommended.
// This method will ask the user whether or not to allow access if they haven't visited this website.
// Also, it will request user to unlock the wallet if the wallet is locked.
await window.keplr.enable(chainId);

const offlineSigner = window.getOfflineSigner(chainId);
**const enigmaUtils = window.getEnigmaUtils(chainId);**

// You can get the address/public keys by `getAccounts` method.
// It can return the array of address/public key.
// But, currently, Keplr extension manages only one address/public key pair.
// XXX: This line is needed to set the sender address for SigningCosmosClient.
const accounts = await offlineSigner.getAccounts();

// Initialize the gaia api with the offline signer that is injected by Keplr extension.
const cosmJS = new SigningCosmWasmClient(
    "https://lcd-secret.keplr.app/rest",
    accounts[0].address,
    offlineSigner,
    **enigmaUtils**
);
```

#### Suggest Adding SNIP-20 Tokens to Keplr <a href="#suggest-adding-snip-20-tokens-to-keplr" id="suggest-adding-snip-20-tokens-to-keplr"></a>

```javascript
async suggestToken(chainId: string, contractAddress: string): Promise<void>
```

The webpage can request the user permission to add a SNIP-20 token to Keplr's token list. Will throw an error if the user rejects the request. If a SNIP-20 with the same contract address already exists, nothing will happen.

#### Get SNIP-20 Viewing Key <a href="#get-snip-20-viewing-key" id="get-snip-20-viewing-key"></a>

```javascript
getSecret20ViewingKey(
    chainId: string,
    contractAddress: string
): Promise<string>;
```

Returns the viewing key of a SNIP-20 token registered in Keplr. If the SNIP-20 of the contract address doesn't exist, it will throw an error.

#### Interaction Options

You can use Keplr native API’s to set interaction options even when using SecretJS. Please refer to [this section](https://docs.keplr.app/api/#interaction-options).\
