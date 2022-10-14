# Tutorials

### React로 프로젝트 시작하기

#### Node.js와 NPM 설치하기

{% embed url="https://nodejs.org/en/" %}

위의 사이트에서 Node.js를 설치합니다. NPM은 Node.js를 설치하면 자동으로 함께 설치됩니다.

설치를 완료하였다면 아래의 명령어를 실행하여 잘 설치되었는지 확인합니다.

```javascript
$ node -v
$ npm -v
```

#### VSCode에서 React 프로젝트 생성

아래 명령어를 원하는 위치의 디렉토리에서 실행시켜 React 프로젝트를 생성합니다.

```javascript
$ npm install -g create-react-app
$ create-react-app {PROJECT_NAME}
```

이 튜토리얼에선 `PROJECT_NAME`을 **`keplr-app`**으로 하겠습니다.

프로젝트를 생성하고 난 뒤 ‘keplr-app’ 디렉토리 구조는 다음과 같습니다.

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

keplr-app 루트 디렉토리로 이동하여 아래와 같은 명령어를 실행시키면 해당 프로젝트의 동작을 웹 브라우저에서 확인할 수 있습니다.

```
$ npm start
```

먼저 아래와 같은 명령어를 실행하여 필요한 패키지들을 설치해줍니다.

```javascript
$ npm i @cosmjs/stargate
$ npm i @keplr-wallet/cosmos
$ npm i @cosmjs/cosmwasm-stargate
$ npm i @cosmjs/proto-signing
$ npm i path
```

⚠️ 만약 아래와 비슷한 오류가 발생한다면 아래 명령어를 추가적으로 실행하십시오.

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

만일 `react-scripts`를 downgrade하는 것을 원치 않는다면 아래의 링크를 참조하여 적절한 조치를 취하십시오.

{% embed url="https://stackoverflow.com/questions/70398678/i-tried-to-polyfill-modules-in-webpack-5-but-not-working-reactjs" %}

이제 React로 프로젝트를 시작할 준비가 끝났습니다. 아래 튜토리얼의 예제 코드를 참조하여 원하는 기능을 구현해보세요!

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
