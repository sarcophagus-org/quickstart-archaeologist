## Table of Contents

1. [Prerequisites](#prerequisites)
2. [Setup Instructions](#setup-instructions)
3. [Logging](#logging)
4. [CLI](#cli)
5. [Running on multiple networks](#running-on-multiple-networks)
6. [Updating the service](#updating-the-service)
7. [Restarting the service](#restarting-the-service)
8. [Troubleshooting](#troubleshooting)

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

- The provider url should be a websocket url. For example, for infura, it would be `wss://mainnet.infura.io/ws/v3/<project_id>`
- _If running on a testnet, then you will need its corresponding Testnet Tokens + Testnet SARCO._

The following Ethereum testnets are currently supported:

- Goerli: (chain id: 5)
- Sepolia (chain id: 11155111)

## Setup Instructions (for Ethereum Mainnet)

1. Once your VPS is setup, connect to your droplet using the CLI:

   > `ssh root@>droplet-ip<`

2. Once logged into your VPS droplet, you will need to 'apt-get install' git, docker, and docker-compose

3. Once setup is complete, Clone this repo:

   > `git clone https://github.com/sarcophagus-org/quickstart-archaeologist && cd quickstart-archaeologist`

4. Copy example env file.

   > `cp .env.example .env`

5. Fill out the `.env` file values. _warning: do not alter the name of the file or it will not be recognized_

   > `nano .env`

   - If you do not already have one, generate a BIP39 seed: `docker compose run seed-gen`

   - Copy this value to your env file as `ENCRYPTION_MNEMONIC`

6. Create blank peer ID file.

   > `touch peer-id.json`

7. **If you have not yet registered your archaeologist:**

   > `docker compose run register`

   _(or `docker-compose` for older versions of docker compose)_

   Follow the instructions to register your archaeologist. A peer ID will automatically be generated for you.

   ***

   **Notes on Registration:**

   - It is recommended you set your free bond to at least a 10x multiple of your digging fee (i.e. if `MIN_DIGGING_FEE=5`, set your `FREE_BOND` to at least `50`).
   - When your free bond drops below your minimum digging fee, you will no longer be able to accept new jobs or appear in the embalmer web application list.
   - See CLI instructions for updating these values after registration.

8. Run the service in the background

   > `docker compose up archaeologist -d`

9. You can verify your service is registered correctly by visiting https://app.dev.sarcophagus.io/archaeologists

- Please allow up to a minute for the archaeologist list to populate.

## Logging

### To view the logs

Run `docker container ls` and grab the container ID of the archaeologist service

Run `docker logs <container-id> --follow` to see realtime logs

OR

Run `docker logs <container-id> --since 12h` to view logs over the last 12 hours.

Replace `12h` as needed to filter logs over more or less time. E.g. - `30m` for 30 minutes.

## CLI

A CLI is provided for running additional commands for your service, such as updating profile values and claiming rewards.

To run the CLI:

1. If the service is not started, start the service with `docker compose up archaeologist -d`,
2. Jump into the container with: `docker compose exec -it archaeologist sh`
3. Run `cli help` for available commands, or `cli help <command>` for help with a given command.

#### Examples

You will need to jump into the container to run these commands: `docker compose exec -it archaeologist sh`

**Update Profile**

This will update your domain + peerID automatically

> `cli update -u`

> `exit`

**Deposit 100 SARCO to free bond**

> `cli update -f 100`

> `exit`

**View Profile**

> `cli view -p`

> `exit`

**Claim Rewards**

> `cli claim`

> `exit`

**Withdraw 5 SARCO from Free Bond**

> `cli free-bond -w 5`

> `exit`

## Running on multiple networks

All commands above will run your archaeologist on the Ethereum mainnet.

To run your archaeologist node on another network, you will need a libp2p peer ID for each network you intend to run on.
The following networks are currently supported:

### Goerli

Make sure `GOERLI_PROVIDER_URL` is set in your `.env` file to an appropriate goerli provider URL: `nano .env`
Run `touch peer-id-goerli.json` in the root directory of this repo.
Run `docker compose run register-goerli` to register your archaeologist on the Goerli testnet.
Run `docker compose up archaeologist-goerli -d` to start the service on the Goerli testnet.

### Sepolia

Make sure `SEPOLIA_PROVIDER_URL` is set in your `.env` file to an appropriate sepolia provider URL: `nano .env`
Run `touch peer-id-sepolia.json` in the root directory of this repo.
Run `docker compose run register-sepolia` to register your archaeologist on the Sepolia testnet.
Run `docker compose up archaeologist-sepolia -d` to start the service on the Sepolia testnet.

### Targetting multiple networks:

While you can individually start up on each network using the above instructions, there's also provision to run on common combinations of multiple networks at once.

Remember to complete the prerequisite setup for each network you intend to run on, if you haven't done so already:

- Set the appropriate provider URLs in your `.env` file.
- Register your archaeologist.
- Create a peer ID file for each network you intend to run on.

#### All supported networks

`COMPOSE_PROFILES=all-networks docker compose up -d`

#### Ethereum Mainnet + All supported Ethereum testnets

`COMPOSE_PROFILES=eth docker compose up -d`

#### All supported Ethereum testnets

`COMPOSE_PROFILES=eth-test docker compose up -d`

#### Ethereum Mainnet + Goerli

`COMPOSE_PROFILES=eth-goerli docker compose up -d`

#### Ethereum Mainnet + Sepolia

`COMPOSE_PROFILES=eth-sepolia docker compose up -d`

## Updating the service

To update the service to the latest version:<br>

> `COMPOSE_PROFILES=all-networks docker compose stop`

> `COMPOSE_PROFILES=all-networks docker compose pull`

- Start your service again using any of the methods above depending on which networks (or combination of networks) you want to run on.

## Restarting the service

To restart the service:<br>

> `docker compose <container-selector> stop`

> `docker compose up <container-selector> -d`

Replace `<container-selector>` with the name of the container you want to restart. See instructions above.

---

## Troubleshooting

Below are some things to do to ensure your archaeologist is running correctly.

### Server ports

Ports 443 and 80 must be open on your server.

If using Uncomplicated Firewall on linux, you can use the following to open these ports:

> `sudo ufw allow 443/tcp; sudo ufw allow 80/tcp; sudo ufw enable`

### Domain A Record

Your domain must have an A record pointing at the IP address of your server that the archaeologist service is running on.

https://www.nslookup.io/website-to-ip-lookup

Use this tool to confirm that your domain is pointed correctly.

### Test Websocket Connection

https://www.piesocket.com/websocket-tester

Test that your archaeologist can have websocket connection open by entering your websocket address in this format:

> `wss://<domain>/p2p/<libp2p peerID>`

To get your domain and peerID, run:

> `docker compose exec -it archaeologist sh`

> `cli view -p`

The `PeerId` has the domain and libp2p peerID concatenated with a `:`. So the format is: `<domain>:<peerID>`.

An example would look like:

> `wss://wss.encryptafile.com/p2p/12D3KooWNpTFhjxvvKhzi6AiKk7ByroGpQ8YKXAy85oLJnrfjCst`

### Archaeologist not showing in the web application

1. Ensure you have registered your archaeologist (see setup instructions).
2. Make sure your free bond is larger than your minimum digging fee. You will not show up in the web application if you do not have enough free bond posted to accept new jobs.
3. Ensure your domain is pointed at the IP address of your server using:
4. See if any errors appear in the logs of either your archaeologist service, or the SSL service

**Archaeologist Logs**

> `docker ps` // get container ID of ghcr.io/sarcophagus-org/sarcophagus-v2-archaeologist-service:latest

> `docker logs <container_id>`

**SSL cert logs**

> `docker ps` // get container ID of nginxproxy/acme-companion

> `docker logs <container_id>`
