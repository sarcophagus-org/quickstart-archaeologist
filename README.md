
## Table of Contents

Linux OS

1. [Prerequisites for Linux](#prerequisites-linux)
2. [Setup Instructions Linux](#setup-instructions-linux)
3. [Logging](#logging)
4. [CLI](#cli)
5. [Updating the service](#updating-the-service)
6. [Restarting the service](#restarting-the-service)

Windows OS

7. [Prerequisites for Windows](#prerequisites-windows)
8. [Setup Instructions for Windows](#setup-instructions-windows)
9. [Logging for Windows](#logging-windows)
10. [CLI for Windows](#cli-windows)
11. [Operations for Windows](#operations-windows)

Any OS

12. [Troubleshooting](#troubleshooting) 


# Prerequisites Linux:
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

_If running on Goerli (chain id = 5), then you will need Goerli ETH + Goerli SARCO._

- A registered domain name [pointed at your server's ip address](https://www.servers.com/support/knowledge/dedicated-servers/how-to-point-your-domain-name-to-dedicated-servers-ip-address#:~:text=To%20point%20your%20domain%20name%20to%20your%20dedicated%20server's%20public,on%20the%20domain's%20name%20servers.) 
---

## Setup Instructions Linux

1. Clone this repo:

   `git clone https://github.com/sarcophagus-org/quickstart-archaeologist && cd quickstart-archaeologist`

2. Copy example env file.

   `cp .env.example .env`

3. Fill out the env file values.  *warning: do not alter the name of the file or it will not be recognized*

   `nano.env` 
  
  or
- To generate a BIP39 seed offline, run: `COMPOSE_PROFILES=seed docker compose run seed-gen`
- Copy this value to your env file

4. Create blank peer ID file.

   `touch peer-id.json`

5. **If you have not yet registered your archaeologist:**

   > `COMPOSE_PROFILES=register docker compose run register`  
   
   _(or `docker-compose` for older versions of docker compose)_

   Follow the instructions to register your archaeologist. A peer ID will automatically be generated for you.

   ---
   
   **Notes on Registration:**
   - It is recommended you set your free bond to at least a 10x multiple of your digging fee. (I.e. if `MIN_DIGGING_FEE=5`, set your `FREE_BOND` to at least `50`.
   - When your free bond drops below your minimum digging fee, you will no longer be able to accept new jobs or appear in the embalmer web application list.
   - See CLI instructions for updating these values after registration.

6. Run the service in the background

   > `COMPOSE_PROFILES=service docker compose up -d`
   
7. You can verify your service is registered correctly by visiting https://dev-sarcophagus.netlify.app/archaeologists
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
1. If the service is not started, start the service with `docker compose up -d`,
2. Jump into the container with: `docker compose exec -it archaeologist sh`
3. Run `cli help` for available commands, or `cli help <command>` for help with a given command.

#### Examples
**Update Profile**
```
// this will update your domain + peerID automatically
docker compose exec -it archaeologist sh
cli update -u
exit
```

**Deposit 100 SARCO to free bond**
```
docker compose exec -it archaeologist sh
cli update -f 100
exit
```

**View Profile**
```
docker compose exec -it archaeologist sh
cli view -p
exit
```

**Claim Rewards**
```
docker compose exec -it archaeologist sh
cli claim
exit
```

**Withdraw 5 SARCO from Free Bond**
```
docker compose exec -it archaeologist sh
cli free-bond -w 5
exit
```

### Updating the service
To update the service to the latest version:<br>
```
COMPOSE_PROFILES=service docker compose stop
COMPOSE_PROFILES=service docker compose pull
COMPOSE_PROFILES=service docker compose up -d
```

### Restarting the service
To restart the service:<br>
```
COMPOSE_PROFILES=service docker compose stop
COMPOSE_PROFILES=service docker compose up -d
```



## Setup Instructions Windows

## Prerequisites Windows: 
- Git Bash for Windows CLI- https://gitforwindows.org/
- Running a server(VPS) with the following installed:
  - git
  - [docker (>= 20.0)](https://www.simplilearn.com/tutorials/docker-tutorial/how-to-install-docker-on-ubuntu)
  - [docker-compose (>= 2.0.0)](https://docs.docker.com/compose/install/linux/#install-the-plugin-manually)
  - Minimum requirements: 4GB hard drive space, 512MB RAM
  - An example of a server with these prerequisites already installed: [https://marketplace.digitalocean.com/apps/docker](https://marketplace.digitalocean.com/apps/docker)
  - **Note:** Server must have ports 80 and 443 open
- ETH wallet (private key) with:
  - ETH balance (for signing transactions)
  - SARCO balance (for bonding your archaeologist to curses)
- BIP39 compatible mnemonic (see **Setup Instructions** or generate a new one here: [https://iancoleman.io/bip39/](https://iancoleman.io/bip39/))
- RPC URL (Infura, Alchemy, etc.)

##Setup Instructions

1. Once your VPS is setup, and you have Gitbash installed, connect to your droplet using the gitbash CLI:

  'ssh root@>droplet-ip<'
  
2. Once logged into your VPS droplet, you will need to 'apt-get install' git, docker, and docker-compose
 
3. Once setup is complete, Clone this repo:

   `git clone https://github.com/sarcophagus-org/quickstart-archaeologist && cd quickstart-archaeologist`

4. Copy example env file.

   `cp .env.example .env`

5. Fill out the env file values.  *warning: do not alter the name of the file or it will not be recognized*

  `nano.env`
  
  or
- To generate a BIP39 seed offline, run: `COMPOSE_PROFILES=seed docker compose run seed-gen`
- Copy this value to your env file

6. Create blank peer ID file.

   `touch peer-id.json`

7. **If you have not yet registered your archaeologist:**

   > `COMPOSE_PROFILES=register docker-compose run register`  
   
   _(or `docker-compose` for older versions of docker compose)_

   Follow the instructions to register your archaeologist. A peer ID will automatically be generated for you.

   ---
   
   **Notes on Registration:**
   - It is recommended you set your free bond to at least a 10x multiple of your digging fee. (I.e. if `MIN_DIGGING_FEE=5`, set your `FREE_BOND` to at least `50`.
   - When your free bond drops below your minimum digging fee, you will no longer be able to accept new jobs or appear in the embalmer web application list.
   - See CLI instructions for updating these values after registration.

8. Run the service in the background

   > `COMPOSE_PROFILES=service docker-compose up -d`
   
9. You can verify your service is registered correctly by visiting https://app.dev.sarcophagus.io/archaeologists
- Please allow up to a minute for the archaeologist list to populate.

## CLI Windows
A CLI is provided for running additional commands for your service, such as updating profile values and claiming rewards.

To run the CLI: 
1. If the service is not started, start the service with `docker compose up -d`,
2. Jump into the container with: `docker compose exec -it archaeologist sh`
3. Run `cli help` for available commands, or `cli help <command>` for help with a given command.

#### Operations Windows
**Update Profile**
```
// this will update your domain + peerID automatically
docker container ls
docker exec >container id< -cli update -u
```

**Deposit 100 SARCO to free bond**
```
docker container ls
docker exec >container id< cli update -f 100
```

**View Profile**
```
docker container ls
docker exec >container id< cli view -p
```

**Claim Rewards**
```
docker container ls
docker exec >container id< cli claim
```

**Withdraw 5 SARCO from Free Bond**
```
docker container ls
docker exec >container id< cli free-bond -w 5
```

### Updating the service
To update the service to the latest version:<br>
```
COMPOSE_PROFILES=service docker-compose stop
COMPOSE_PROFILES=service docker-compose pull
COMPOSE_PROFILES=service docker-compose up -d
```

### Restarting the service
To restart the service:<br>
```
COMPOSE_PROFILES=service docker-compose stop
COMPOSE_PROFILES=service docker-compose up -d
```


## Troubleshooting
Below are some things to do to ensure your archaeologist is running correctly.

### Server ports
Ports 443 and 80 must be open on your server.

If using Uncomplicated Firewall on linux, you can use the following to open these ports:
```
sudo ufw allow 443/tcp; sudo ufw allow 80/tcp; sudo ufw enable
```

### Domain A Record
Your domain must have an A record pointing at the IP address of your server that the archaeologist service is running on.

https://www.nslookup.io/website-to-ip-lookup

Use this tool to confirm that your domain is pointed correctly.

### Test Websocket Connection
https://www.piesocket.com/websocket-tester

Test that your archaeologist can have websocket connection open by entering your websocket address in this format:

`wss://<domain>/p2p/<libp2p peerID>`

To get your domain and peerID, run:

```
docker compose exec -it archaeologist sh
cli view -p
```

The `PeerId` has the domain and libp2p peerID concatenated with a `:`. So the format is: `<domain>:<peerID>`.

An example would look like:
`wss://wss.encryptafile.com/p2p/12D3KooWNpTFhjxvvKhzi6AiKk7ByroGpQ8YKXAy85oLJnrfjCst`


### Archaeologist not showing in the web application
1. Ensure you have registered your archaeologist (see setup instructions). 
2. Make sure your free bond is larger than your minimum digging fee. You will not show up in the web application if you do not have enough free bond posted to accept new jobs.
3. Ensure your domain is pointed at the IP address of your server using: 
4. See if any errors appear in the logs of either your archaeologist service, or the SSL service

```
**Archaeologist Logs**
docker container ls   // get container ID of ghcr.io/sarcophagus-org/sarcophagus-v2-archaeologist-service:latest
docker logs <container_id>
```

```
**SSL cert logs**
docker container ls   // get container ID of nginxproxy/acme-companion
docker logs <container_id>
```

### Updating your domain after registering
If you update your domain after registering, you will need to update the archaeologist profile and restart your service.

```
// Update the archaeologist by depositing 1 free bond
docker compose exec -it archaeologist sh
cli update -f 1
exit

// restart archaeologist service
COMPOSE_PROFILES=service docker compose stop
COMPOSE_PROFILES=service docker compose up -d
```
