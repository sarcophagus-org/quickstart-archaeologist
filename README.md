## Setup Instructions

### Prerequisites:
- Running a [server with bash + Docker installed](https://marketplace.digitalocean.com/apps/docker). 
- ETH wallet (private key) with:
  - ETH balance (for signing transactions)
  - SARCO balance (for bonding your archaeologist to curses)
- RPC URL (Infura, Alchemy, etc.)

_If running on Goerli (chain id = 5), then you will need Goerli ETH + Goerli SARCO._

### Initial Setup Instructions

Copy example env file, and fill out the ETH_PRIVATE_KEY and PROVIDER_URL fields.
> `cp .env.example .env`

Create blank peer ID file.
> `touch peer-id.json`

Run the service:
> docker-compose up

### Registering your archaeologist
If this is your first time running the service, you will be prompted to register your archaeologist.

### Further instructions

To start the service at any point after running the initial setup instructions, just use:
> `docker-compose up`