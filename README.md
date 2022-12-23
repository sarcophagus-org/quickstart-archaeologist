## Setup Instructions

### Prerequisites:
- Running a server with the following installed:
  - git
  - [docker (>= 20.0)](https://www.simplilearn.com/tutorials/docker-tutorial/how-to-install-docker-on-ubuntu)
  - [docker compose (>= 2.0.0)](https://docs.docker.com/compose/install/linux/#install-the-plugin-manually)
  - An example of a server with these prerequisites already installed: [https://marketplace.digitalocean.com/apps/docker](https://marketplace.digitalocean.com/apps/docker)
- ETH wallet (private key) with:
  - ETH balance (for signing transactions)
  - SARCO balance (for bonding your archaeologist to curses)
- BIP39 compatible mnemonic (generate a new one here: [https://iancoleman.io/bip39/](https://iancoleman.io/bip39/))
- RPC URL (Infura, Alchemy, etc.)

_If running on Goerli (chain id = 5) or Sepolia (chain id = 11155111), then you will need Goerli/Sepolia ETH + Goerli/Sepolia SARCO._

- A registered domain name [pointed at your server's ip address](https://www.servers.com/support/knowledge/dedicated-servers/how-to-point-your-domain-name-to-dedicated-servers-ip-address#:~:text=To%20point%20your%20domain%20name%20to%20your%20dedicated%20server's%20public,on%20the%20domain's%20name%20servers.) 
---

### Initial Setup Instructions

1. Clone this repo:

   `git clone https://github.com/sarcophagus-org/quickstart-archaeologist`

2. Copy example env file, and fill out the env file values. See example env for explanation

   `cp .env.example .env`

3. Create blank peer ID file.

   `touch peer-id.json`

4. **If you have not yet registered your archaeologist:**

   > `COMPOSE_PROFILES=register docker compose run register`  
   
   _(or `docker-compose` for older versions of docker compose)_

   Follow the instructions to register your archaeologist. A peer ID will automatically be generated for you.

5. Run the service

   > `COMPOSE_PROFILES=service docker compose up`

### CLI
A CLI is provided for running additional commands for your service, such as updating profile values and claiming rewards.

To run the CLI: 
1. If the service is not started, start the service with `docker compose up`,
2. Jump into the container with: `docker compose exec archaeologist sh`
3. Run `cli help` for available commands, or `cli help <command>` for help with a given command.

### Further instructions
To update the service to the latest version:
`docker-compose pull`
