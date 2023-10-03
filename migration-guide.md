### Overview
The latest updates allow you to run your archaeologist node on multiple networks. The following networks are supported:
- ETH Mainnet
- ETH Goerli
- ETH Sepolia
- Polygon Mumbai

Polygon Mainnet will be available soon.

**Your `.env` file will require some changes for the archaeologist to function.**

See the `.env.example` for the latest format of environment variables. These changes are described below.

Any networks that you do not want to run on, you can leave their respective environment variables blank and remove the networks' chain ID(s) from the list of CHAIN_IDs.

### CHAIN_IDS
In your `.env` file, set `CHAIN_IDS` to the chain ids of the networks you would like to run on.
For example, if you would like to run on ETH mainnet and ETH goerli, you would set `CHAIN_IDS="1,5"`.

### PROVIDER_URLS
See the `.env.example` for the new formatting of provider URLs.

You will need to add provider urls for any networks you want to run your archaeologist on.

The archaeologist service has been updated to use a websocket provider.
Your provider url will need to be updated to continue running. It should now be in the `wss:://` format.
For example, for infura mainnet, it would look like this: `wss://mainnet.infura.io/ws/v3/<your-infura-api-key>`

#### Remove PROVIDER_URL
Your current `PROVIDER_URL` is no longer in use, this can be removed.

### ENCRYPTION_MNEMONICS
See the `.env.example` for the new formatting of encryption mnemonics.

You should copy your current ENCRYPTION_MNEMONIC to the correct network mnemonic.
For example, if you are running on mainnet, copy the ENCRYPTION_MNEMONIC value to MAINNET_ENCRYPTION_MNEMONIC

The ENCRYPTION_MNEMONIC can then be removed from your `.env` file.

Add encryption mnemonics for any other networks you want to run your archaeologist. 

**Important note:** -- each encryption mnemonic must be unique, do not use the same mnemonic across networks.

### Update your node environment by running these commands in order:
- `COMPOSE_PROFILES=service docker compose down --remove-orphans`
- `docker image prune`
- `git fetch origin develop && git checkout develop && git pull origin develop`
- `COMPOSE_PROFILES=service docker compose pull`

If you want to register on more networks than you were previously, see [README](./README.md#running-on-multiple-networks)
for instructions on registering on multiple networks.

Once you are registered on all the networks you would like, run:
`COMPOSE_PROFILES=service NETWORK=all docker compose up -d`

### Setting up notifications:

The quickstart repository readme has been updated to include instructions on setting up optional notifications.
See the [README](./README.md#notifications) if you would like to set these up.

