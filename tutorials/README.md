# Tutorials

### Start a project with React

#### Install Node.js and NPM

{% embed url="https://nodejs.org/en/" %}

Install Node.js from the site above. NPM is automatically installed together when Node.js is installed.

If you have completed the installation, run the command below to verify that it is installed well.

```javascript
$ node -v
$ npm -v
```

#### Create a React Project in VSCode

Create a React project by executing the command below from the directory in the desired location.

```javascript
$ npm install -g create-react-app
$ create-react-app {PROJECT_NAME}
```

In this tutorial, we'll use `PROJECT_NAME` as **`keplr-app`**.

After creating the project, the 'keplr-app' directory structure is as follows.

```javascript
keplr-app
├── README.md
├── node_modules
├── package.json
├── package-lock.json
├── .gitignore
├── public
│   ├── favicon.ico
│   ├── index.html
│   └── manifest.json
└── src
    ├── App.css
    ├── App.js
    ├── App.test.js
    ├── index.css
    ├── index.js
    ├── logo.svg
    └── reportWebVitals.js
```

Go to the keplr-app root directory and run the command below to see the behavior of the project in a web browser.

```
$ npm start
```

First, run the following command to install the necessary packages.

```javascript
$ npm i @cosmjs/stargate
$ npm i @keplr-wallet/cosmos
$ npm i @cosmjs/cosmwasm-stargate
$ npm i @cosmjs/proto-signing
$ npm i path
```

⚠ If you encounter an error similar to the one below, run the following additional commands:

```javascript
Module not found: Error: Can't resolve 'crypto' in {file_path}

BREAKING CHANGE: webpack < 5 used to include polyfills for node.js core modules by default.
This is no longer the case. Verify if you need this module and configure a polyfill for it.

If you want to include a polyfill, you need to:
    - add a fallback 'resolve.fallback: { "crypto": require.resolve("crypto-browserify") }'
    - install 'crypto-browserify'
If you don't want to include a polyfill, you can use an empty module like this:
    resolve.fallback: { "crypto": false }
```

```javascript
$ npm uninstall react-scripts
$ npm install react-scripts@4.0.3
```

If you do not wish to downgrade the `react-scripts`, please refer to the link below and take appropriate action.

{% embed url="https://stackoverflow.com/questions/70398678/i-tried-to-polyfill-modules-in-webpack-5-but-not-working-reactjs" %}

You are now ready to start your project with React. Refer to the example code in the tutorial below to implement the features you want!

{% content-ref url="get-balance.md" %}
[get-balance.md](get-balance.md)
{% endcontent-ref %}

{% content-ref url="send-tokens.md" %}
[send-tokens.md](send-tokens.md)
{% endcontent-ref %}

{% content-ref url="delegate-tokens.md" %}
[delegate-tokens.md](delegate-tokens.md)
{% endcontent-ref %}

{% content-ref url="get-contract.md" %}
[get-contract.md](get-contract.md)
{% endcontent-ref %}
