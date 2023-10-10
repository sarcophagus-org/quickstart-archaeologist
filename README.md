## Table of Contents

1. [Prerequisites](#prerequisites)
2. [Setup Instructions](#setup-instructions)
3. [Logging](#logging)
4. [CLI](#cli)
5. [Running on multiple networks](#running-on-multiple-networks)
6. [Updating the service](#updating-the-service)
7. [Restarting the service](#restarting-the-service)
8. [Notifications](#notifications)
9. [Troubleshooting](#troubleshooting)

## Prerequisites:

_For Windows users, steps are the same, but install GitBash (or similar) for a CLI environment to operate from:_ https://gitforwindows.org/

- Running a server with the following installed:
  - git
  - [docker (>= 20.0)](https://www.simplilearn.com/tutorials/docker-tutorial/how-to-install-docker-on-ubuntu)
  - [docker compose (>= 2.0.0)](https://docs.docker.com/compose/install/linux/#install-the-plugin-manually)
  - Minimum requirements: 4GB hard drive space, 512MB RAM
  - An example of a server with these prerequisites already installed: [https://marketplace.digitalocean.com/apps/docker](https://marketplace.digitalocean.com/apps/docker)
  - **Note:** Server must have ports 80 and 443 open
- ETH wallet (private key) with:
  - ETH balance (for signing transactions)
  - SARCO balance (for bonding your archaeologist to curses)
- BIP39 compatible mnemonic (see **Setup Instructions** or generate a new one here: [https://iancoleman.io/bip39/](https://iancoleman.io/bip39/))
- RPC URL (Infura, Alchemy, etc.)
- A registered domain name [pointed at your server's ip address](https://www.servers.com/support/knowledge/dedicated-servers/how-to-point-your-domain-name-to-dedicated-servers-ip-address#:~:text=To%20point%20your%20domain%20name%20to%20your%20dedicated%20server's%20public,on%20the%20domain's%20name%20servers.)

**NOTE**:

- Provider urls should be a websocket url. For example, for infura, it would be `wss://mainnet.infura.io/ws/v3/<project_id>`
- _For each network you are running on, you will need its corresponding Network Tokens + Network SARCO._

## Setup Instructions

1. Once your VPS is setup, connect to your droplet using the CLI:

   - `ssh root@>droplet-ip<`

2. Once logged into your VPS droplet, you will need to 'apt-get install' git, docker, and docker-compose

3. Once setup is complete, Clone this repo:

   - `git clone https://github.com/sarcophagus-org/quickstart-archaeologist && cd quickstart-archaeologist`

4. Copy example env file.

   - `cp .env.example .env`

5. Fill out the `.env` file values. _warning: do not alter the name of the file or it will not be recognized_

   - `nano .env`

   **(on Encryption Mnemonic:)**

   - If you do not already have one, you can generate a BIP39 seed: `COMPOSE_PROFILES=seed-gen docker compose run seed-gen`

   - Copy this value to your env file.

   - You will need a unique mnemonic for each network you intend to run on.

6. Create blank peer ID file.

   - `touch peer-id.json`

7. **If you have not yet registered your archaeologist:**

   - `COMPOSE_PROFILES=register NETWORK=<network> docker compose run register`

   Replace `<network>` with a network/chain-id from your CHAIN_IDS env variable. You will need to rerun this command separately for each network indicated in your `.env` file to register yourself as an archaologist on each network.

   _(or `docker-compose` for older versions of docker compose)_

   Follow the instructions to register your archaeologist. A peer ID will automatically be generated for you.

   ***

   **Notes on Registration:**

   - It is recommended you set your free bond to at least a 10x multiple of your digging fee (i.e. if `MIN_DIGGING_FEE=5`, set your `FREE_BOND` to at least `50`).
   - When your free bond drops below your minimum digging fee, you will no longer be able to accept new jobs or appear in the embalmer web application list.
   - See CLI instructions for updating these values after registration.

8. Run the service in the background

   - `COMPOSE_PROFILES=service NETWORK=<network> docker compose up archaeologist -d`

Replace `<network>` with a network/chain-id if you would like to run on one of your configured networks, or `all` to run on all networks indicated in your `.env` file.

9. You can verify your service is registered correctly by visiting https://app.dev.sarcophagus.io/archaeologists. Make your wallet is connected to the network you are running on.

- Please allow up to a minute for the archaeologist list to populate.

## Logging

### To view the logs

Run `docker ps` and grab the container ID of the archaeologist service. Look under the `NAMES` column. Your archaeologist docker container should have the name format: `<directory-name>-archaeologist-1`.

Run `docker logs <container-id> --follow` to see realtime logs

OR

Run `docker logs <container-id> --since 12h` to view logs over the last 12 hours.

Replace `12h` as needed to filter logs over more or less time. E.g. - `30m` for 30 minutes.

## CLI

A CLI is provided for running additional commands for your service, such as updating profile values and claiming rewards.

To run the CLI:

1. If the service is not started, start the service with `COMPOSE_PROFILES=service NETWORK=<network> docker compose up archaeologist -d`.

Replace `<network>` with a network/chain-id if you would like to run on one of your configured networks, or `all` to run on all networks indicated in your `.env` file.

2. Jump into the container with: `docker compose exec -it archaeologist sh`
3. Run `cli help` for available commands, or `cli help <command>` for help with a given command.

For each CLI command, you may choose to omit the `--network <network>` flag if your node is running on just one network.

If running on more than one network, you will need to specify the network you would like to run the CLI command on.

#### Examples

You will need to jump into the container to run these commands: `docker compose exec -it archaeologist sh`

**Update Profile**

- `cli update -u --network <network>`

_This will update your domain + peerID automatically._

**Deposit 100 SARCO to free bond**

- `cli update -f 100 --network <network>`

**View Profile**

- `cli view -p --network <network>`

**Claim Rewards**

- `cli claim --network <network>`

**Withdraw 5 SARCO from Free Bond**

- `cli free-bond -w 5 --network <network>`

**Exit the CLI**

- `exit`

## Running on multiple networks

To run your archaeologist node on multiple networks, you will need to:

- Set `CHAIN_IDS` to a comma-separated list of networks to run on. See `.env.example`.
- Set the appropriate provider URLs for each network you intend to run on. See `.env.example`.
- Set the appropriate encryption mnemonics for each network you intend to run on. See `.env.example`.
- After configuring each network as described below, you may start the service with: `COMPOSE_PROFILES=service NETWORK=<network> docker compose up -d`.

Replace `<network>` with a network/chain-id, or `all` to run on all networks.

The following networks are currently supported:

### Ethereum Mainnet

- Make sure `MAINNET_PROVIDER_URL` is set in your `.env` file to an appropriate goerli provider URL.
- Make sure `MAINNET_ENCRYPTION_MNEMONIC` is set in your `.env` file.
- Run `COMPOSE_PROFILES=register NETWORK=mainnet docker compose run register` to register your archaeologist on the Ethereum mainnet network.

### Goerli

- Make sure `GOERLI_PROVIDER_URL` is set in your `.env` file to an appropriate goerli provider URL.
- Make sure `GOERLI_ENCRYPTION_MNEMONIC` is set in your `.env` file.
- Run `COMPOSE_PROFILES=register NETWORK=goerli docker compose run register` to register your archaeologist on the Goerli testnet.

### Sepolia

- Make sure `SEPOLIA_PROVIDER_URL` is set in your `.env` file to an appropriate sepolia provider URL.
- Make sure `SEPOLIA_ENCRYPTION_MNEMONIC` is set in your `.env` file.
- Run `COMPOSE_PROFILES=register NETWORK=sepolia docker compose run register` to register your archaeologist on the Sepolia testnet.

### Base Goerli

- Make sure `BASE_GOERLI_PROVIDER_URL` is set in your `.env` file to an appropriate BaseGoerli provider URL.
- Make sure `BASE_GOERLI_ENCRYPTION_MNEMONIC` is set in your `.env` file.
- Run `COMPOSE_PROFILES=register NETWORK=baseGoerli docker compose run register` to register your archaeologist on the Base Goerli testnet.

### Polygon Mainnet

- Make sure `POLYGON_MAINNET_PROVIDER_URL` is set in your `.env` file to an appropriate PolygonMainnet provider URL.
- Make sure `POLYGON_MAINNET_ENCRYPTION_MNEMONIC` is set in your `.env` file.
- Run `COMPOSE_PROFILES=register NETWORK=polygon docker compose run register` to register your archaeologist on the Polygon Mainnet network.

### Polygon Mumbai

- Make sure `POLYGON_MUMBAI_PROVIDER_URL` is set in your `.env` file to an appropriate PolygonMumbai provider URL.
- Make sure `POLYGON_MUMBAI_ENCRYPTION_MNEMONIC` is set in your `.env` file.
- Run `COMPOSE_PROFILES=register NETWORK=polygonMumbai docker compose run register` to register your archaeologist on the Polygon Mumbai testnet.

## Updating the service

To update the service to the latest version:<br>

- `COMPOSE_PROFILES=service docker compose stop`

- `COMPOSE_PROFILES=service docker compose pull`

- `COMPOSE_PROFILES=service NETWORK=<network> docker compose up -d`

## Restarting the service

To restart the service:<br>

- `COMPOSE_PROFILES=service docker compose stop`

- `COMPOSE_PROFILES=service NETWORK=<network> docker compose up -d`

## Notifications

You can setup your node environment so that you can receive realtime notifications from your nodes. This is important if you would like to stay on top of the status of your nodes without having to remember to check in often.

To set up notifications, set the following environment variable(s) in your `.env` file:

- `NOTIFICATION_WEBHOOK_URL=<your-webhook-url>`

  You can follow the instructions here to set up a discord webhook url:
  https://support.discord.com/hc/en-us/articles/228383668-Intro-to-Webhooks

Or, to get updates via email:

- `NOTIFICATION_SENDGRID_API_KEY=<your-sendgrid-api-key>`
- `NOTIFICATION_SENDGRID_EMAIL=<the-email-to-receive-updates-on>`
- `NOTIFICATION_SENDGRID_VERIFIED_SENDER=<your-sendgrid-verified-sender>`

You may choose to set all of these to receive notifications via both methods.

---

## Troubleshooting

Below are some things to do to ensure your archaeologist is running correctly.

### Server ports

Ports 443 and 80 must be open on your server.

If using Uncomplicated Firewall on linux, you can use the following to open these ports:

- `sudo ufw allow 443/tcp; sudo ufw allow 80/tcp; sudo ufw enable`

### Domain A Record

Your domain must have an A record pointing at the IP address of your server that the archaeologist service is running on.

https://www.nslookup.io/website-to-ip-lookup

Use this tool to confirm that your domain is pointed correctly.

### Test Websocket Connection

https://www.piesocket.com/websocket-tester

Test that your archaeologist can have websocket connection open by entering your websocket address in this format:

- `wss://<domain>/p2p/<libp2p peerID>`

To get your domain and peerID, run:

- `docker compose exec -it archaeologist sh`

- `cli view -p --network <network>`

The `PeerId` has the domain and libp2p peerID concatenated with a `:`. So the format is: `<domain>:<peerID>`.

An example would look like:

- `wss://wss.encryptafile.com/p2p/12D3KooWNpTFhjxvvKhzi6AiKk7ByroGpQ8YKXAy85oLJnrfjCst`

### Archaeologist not showing in the web application

1. Ensure you have registered your archaeologist on the correct network (see setup instructions).
2. Make sure your free bond is larger than your minimum digging fee. You will not show up in the web application if you do not have enough free bond posted to accept new jobs.
3. Ensure your domain is pointed at the IP address of your server using:
4. See if any errors appear in the logs of either your archaeologist service, or the SSL service

**Archaeologist Logs**

- `docker ps` // get container ID of ghcr.io/sarcophagus-org/sarcophagus-v2-archaeologist-service:latest

- `docker logs <container_id>`

**SSL cert logs**

- `docker ps` // get container ID of nginxproxy/acme-companion

- `docker logs <container_id>`
