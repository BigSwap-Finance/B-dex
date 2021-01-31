# DEX

[![Chat with us on Discord](https://img.shields.io/badge/chat-Discord-blueViolet.svg)](https://discord.gg/JqheZms)
[![CircleCI](https://circleci.com/gh/0xProject/0x-launch-kit-frontend.svg?style=svg)](https://circleci.com/gh/0xProject/0x-launch-kit-frontend)
[![dependencies Status](https://david-dm.org/verisafe/veridex/status.svg)](https://david-dm.org/verisafe/veridex)
[![devDependencies Status](https://david-dm.org/verisafe/veridex/dev-status.svg)](https://david-dm.org/verisafe/veridex?type=dev)
[![Coverage Status](https://coveralls.io/repos/github/VeriSafe/VeriDex/badge.svg?branch=development)](https://coveralls.io/github/VeriSafe/VeriDex?branch=development)

This project is forked from [0x-launch-kit-fronted](https://github/0xproject/0x-launch-kit-frontend) and it have a goal to be the most complete open-source 0x based dex out there. The code here will try to be on sync with the 0x frontend, but with the additional features proposed on the TODO, tests will be include following 0x style.


This repo ships with both an ERC-20 token trading interface and an ERC-721 marketplace interface. However, for now, only improvements on ERC-20 token trading will be done.



With your help, we can be self-sustainable and complete the long list of TODO's. If you want a feature that is not present on the TODO list, please open an issue requesting a feature request.

If you have the URL of an existing relayer, you can use this frontend against it. After installing the dependencies, start the application with this command, replacing `RELAYER_URL` and `RELAYER_WS_URL` with the proper value:

```
REACT_APP_RELAYER_URL='https://RELAYER_URL/' REACT_APP_RELAYER_WS_URL='wss://RELAYER_URL/' yarn start
```

## Usage

Clone this repository and install its dependencies:

You can optionally pass in any relayer endpoint that complies with the [0x Standard Relayer API](https://github.com/0xProject/standard-relayer-api). For example, if you want to show liquidity from 0x API:

```
REACT_APP_RELAYER_URL='https://api.0x.org/sra' REACT_APP_RELAYER_WS_URL='wss://api.0x.org/sra' REACT_APP_NETWORK_ID=1 REACT_APP_CHAIN_ID=1 yarn start
```

### Creating a relayer for development

If you don't have a relayer, you can start one locally for development. First create a `docker-compose.yml` file like this:

```yml
version: '3'
services:
    ganache:
        image: 0xorg/ganache-cli
        ports:
            - '8545:8545'
    backend:
        image: 0xorg/launch-kit-backend:latest
        environment:
            HTTP_PORT: '3000'
            NETWORK_ID: '50'
            CHAIN_ID: '1337'
            WHITELIST_ALL_TOKENS: 'true'
            FEE_RECIPIENT: '0x0000000000000000000000000000000000000001'
            MAKER_FEE_UNIT_AMOUNT: '0'
            TAKER_FEE_UNIT_AMOUNT: '0'
            MESH_ENDPOINT: 'ws://mesh:60557'
        ports:
            - '3000:3000'
    mesh:
        image: 0xorg/mesh:7.2.1-beta-0xv3
        environment:
            ETHEREUM_RPC_URL: 'http://ganache:8545'
            ETHEREUM_CHAIN_ID: 1337
            VERBOSITY: 3
            RPC_ADDR: 'mesh:60557'
            # You can decrease the BLOCK_POLLING_INTERVAL for test networks to
            # improve performance. See https://0x-org.gitbook.io/mesh/ for more
            # Documentation about Mesh and its environment variables.
            BLOCK_POLLING_INTERVAL: '5s'
        ports:
            - '60557:60557'
            - '60558:60558'
            - '60559:60559'
```

and then run `docker-compose up`. This will create three containers: one has a ganache with the 0x contracts deployed and some test tokens, another one has an instance of the [launch kit](https://github.com/0xProject/0x-launch-kit) implementation of a relayer that connects to that ganache and finally a container for [0x-mesh](https://github.com/0xProject/0x-mesh) for order sharing and discovery on a p2p network.

After starting those containers, you can run the following in another terminal. A browser tab will open in the `http://localhost:3001` address. You'll need to connect MetaMask to `localhost:8545`.

```
REACT_APP_RELAYER_URL='http://localhost:3000/v3' REACT_APP_RELAYER_WS_URL='ws://localhost:3000' yarn start
```

```
git clone git@github.com:VeriSafe/veridex.git
cd Veridex
yarn
```

## TODO

This is a detailed list of planned features to add to this DEX (includes VeriDex backend) on long term:

-   [x] List Dex Trades
-   [x] Add troll box using ChatBro
-   [x] Fully configuration of orderbook and sell and buy cards
-   [x] Support multiple wallets, like Portis, Torus etc please see list of planned wallets below,
-   [x] Add mobile support
-   [x] Support to transfer tokens
-   [x] Display prices and total holdings on wallet
-   [x] Display median price
-   [x] Add notifications
-   [x] List descriptions for each project
-   [x] List Market Trades
-   [x] List Markets stats
-   [x] List last prices for each token
-   [x] Add Fiat on Ramp
-   [x] Add 0x Instant to easy buy of assets
-   [x] Adding graphs like Trading View
-   [x] Support for mobile dapp browswers like Enjin and Coinbase
-   [x] Mobile friendly
-   [x] Connect to 0x mesh
-   [x] Adding Account market stats
-   [ ] Click on buy and sell button to auto-fill
-   [ ] Create a costumized front page
-   [x] Order Matching on the Frontend when doing limit orders
-   [x] Page for trading competitions
-   [x] Add instant as standalone
-   [x] Code splitting
-   [ ] Report data to the most known crypto data aggregators (In progress)
-   [x] Theme switcher
-   [x] Dex Wizard
-   [x] Upgrade 0x v3
-   [ ] [i18n](https://github.com/i18next/react-i18next)
-   [x] Add [tour](https://github.com/elrumordelaluz/reactour)
-   [ ] Add crypto price calculator
-   [ ] Add Swap interface
-   [ ] Add Token factory

## Planned Wallets Support

-   [x] [Metamask](https://metamask.io/)
-   [ ] [Torus](https://docs.tor.us/developers/getting-started)
-   [x] [Portis](https://developers.portis.io/)
-   [x] [Fortmatic](https://developers.fortmatic.com/)
-   [ ] [WalletConnect](https://docs.walletconnect.org/)
-   [x] [EnjinWallet](https://enjin.io/products/wallet)
-   [x] [CoinbaseWallet](https://wallet.coinbase.com/)
-   [x] [TrustWallet](https://trustwallet.com)
-   [x] [CipherBrowser](https://www.cipherbrowser.com/)


