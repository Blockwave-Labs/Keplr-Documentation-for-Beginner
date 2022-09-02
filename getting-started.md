# Getting Started

Use this beginner-friendly guide to start building with Keplr.&#x20;

Follow the steps below to get started with Keplr!

## 1. Download Keplr

To develop for Keplr, install Keplr in the browser of your choice on your development machine. [Download here](https://www.keplr.app/)

## 2. Detecting Keplr

You can determine whether Keplr is installed on the user device by checking `window.keplr`. If `window.keplr` returns `undefined` after document.load, Keplr is not installed. There are several ways to wait for the load event to check the status. Refer to the examples below:

You can register the function to `window.onload`:

```javascript
window.onload = async () => {
    if (!window.keplr) {
        alert("Please install keplr extension");
    } else {
        const chainId = "cosmoshub-4";
        
        // Enabling before using the Keplr is recommended.
        // This method will ask the user whether to allow access if they haven't visited this website.
        // Also, it will request that the user unlock the wallet if the wallet is locked.
        await window.keplr.enable(chainId);

        const offlineSigner = window.keplr.getOfflineSigner(chainId);
    
        // You can get the address/public keys by `getAccounts` method.
        // It can return the array of address/public key.
        // But, currently, Keplr extension manages only one address/public key pair.
        // XXX: This line is needed to set the sender address for SigningCosmosClient.
        const accounts = await offlineSigner.getAccounts();

        // Initialize the gaia api with the offline signer that is injected by Keplr extension.
        const cosmJS = new SigningCosmosClient(
            "https://lcd-cosmoshub.keplr.app",
            accounts[0].address,
            offlineSigner,
        );
    }
} 
```

or track the document's ready state through the document event listener:

```javascript
async getKeplr(): Promise<Keplr | undefined> {
    if (window.keplr) {
        return window.keplr;
    }
    
    if (document.readyState === "complete") {
        return window.keplr;
    }

    return new Promise((resolve) => {
        const documentStateChange = (event: Event) => {
            if (
                event.target &&
                (event.target as Document).readyState === "complete"
            ) {
                resolve(window.keplr);
                document.removeEventListener("readystatechange", documentStateChange);
            }
        };
        
        document.addEventListener("readystatechange", documentStateChange);
    });
}
```

There may be multiple ways to achieve the same result, and no preferred method.

## Development Suite

Congrats! You have the basics down!

Now you can dive into the other tools in the Keplr Development Suite.&#x20;

Visit the Tools & Libraries Guide for more info.

### CosmJs

Follow the [CosmJs tutorial](tools-and-libraries/cosmjs.md) to get started.

### Suggest Chain

If you want a front end to request adding new Cosmos SDK based blockchains that isn't natively integrated to Keplr extension, use Suggest Chain feature. Follow the [Suggest Chain tutorial](tools-and-libraries/suggest-chain.md) to get started.

### Specific Features

If you were able to connect Keplr with CosmJs, you may skip to the [Use Keplr with CosmJs ](tools-and-libraries/cosmjs.md)section. While Keplr supports an easy way to connect to CosmJs, there are additional functions specific to Keplr which provides additional features. Check the Specific Features in [here](tools-and-libraries/specific-features.md).

### SecretJs

If you need to use secret-wasm feature, use SecretJs with Keplr. Follow the [SecretJs tutorial ](tools-and-libraries/secretjs.md)to get started.

