## Overview
The latest updates allow you to run your archaeologist node on multiple networks. The following networks are supported:
- ETH Mainnet  (chain: 1)
- Polygon Mainnet (chain: 137)
- ETH Sepolia (chain: 11155111)
- Polygon Mumbai (chain: 80001)

## Index
1. [Env File Updates](#env-file-updates)

2. [Update your Archaeologist](#update-your-archaeologist)

3. [Register your Archaeologist](#register-your-archaeologist)

4. [Start Your Archaeologist](#start-your-archaeologist)

## ENV File Updates
**Your `.env` file will require some changes for the archaeologist to function.**

```
// Pull latest main branch + pull newest package data
git pull origin main
```

After pulling the `main` branch, see the `.env.example` for the latest format of environment variables. These changes are described below.

Any networks that you do not want to run on, you can leave their respective environment variables blank and remove the networks' chain ID(s) from the list of CHAIN_IDs.

### CHAIN_IDS
In your `.env` file, set `CHAIN_IDS` to the chain ids of the networks you would like to run on.
For example, if you would like to run on ETH mainnet, Polygon mainnet and ETH goerli, you would set `CHAIN_IDS="1,137,5"`.

### PROVIDER_URLS
See the `.env.example` for the new formatting of provider URLs.

You will need to add provider urls for any networks you want to run your archaeologist on.

The archaeologist service has been updated to use a websocket provider.
Your provider url will need to be updated to continue running. It should now be in the `wss:://` format.
For example, for infura mainnet, it would look like: `wss://mainnet.infura.io/ws/v3/<your-infura-api-key>`

**Note: Your current `PROVIDER_URL` is no longer needed and can be removed. **

### ENCRYPTION_MNEMONICS
See the `.env.example` for the new formatting of encryption mnemonics.

You should copy your current ENCRYPTION_MNEMONIC to the correct network mnemonic.
For example, if you are running on mainnet, copy the ENCRYPTION_MNEMONIC value to MAINNET_ENCRYPTION_MNEMONIC

The ENCRYPTION_MNEMONIC can then be removed from your `.env` file.

Add encryption mnemonics for any other networks you want to run your archaeologist. 

**Important note:** -- each encryption mnemonic must be unique, do not use the same mnemonic across networks.

## Update your Archaeologist
```
// Destroy + Clean docker images
COMPOSE_PROFILES=service docker compose down --remove-orphans
docker image prune

// pull latest node package
COMPOSE_PROFILES=service docker compose pull
```

## Register your Archaeologist
You can now register your archaeologist on multiple networks. 

You do not need to re-register your archaeologist on any networks your are currently registered on.


**To register:**
```
COMPOSE_PROFILES=register NETWORK=<network> docker compose run register

// `<network>` is the network you would like to register. Available networks are:
mainnet
polygonMainnet
sepolia
polygonMumbai
```

## Start your Archaeologist
```
// All networks - excluding network flag will automatically run all networks added to CHAIN_IDS
`COMPOSE_PROFILES=service docker compose run up -d`

// Single Network
`COMPOSE_PROFILES=service NETWORK=<network> docker compose run up -d`
```

### Setting up notifications:
The quickstart repository readme has been updated to include instructions on setting up optional notifications.
See the [README](./README.md#notifications) if you would like to set these up.
