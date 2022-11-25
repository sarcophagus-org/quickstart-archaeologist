## Setup Instructions

### Prerequisites:
- Running a [server with the following installed](https://marketplace.digitalocean.com/apps/docker):
  - git
  - [docker (>= 20.0)](https://www.simplilearn.com/tutorials/docker-tutorial/how-to-install-docker-on-ubuntu)
  - [docker compose (>= 2.0.0)](https://docs.docker.com/compose/install/linux/#install-the-plugin-manually)
- ETH wallet (private key) with:
  - ETH balance (for signing transactions)
  - SARCO balance (for bonding your archaeologist to curses)
- RPC URL (Infura, Alchemy, etc.)

_If running on Goerli (chain id = 5), then you will need Goerli ETH + Goerli SARCO._

---

### Initial Setup Instructions

1. Clone this repo:

   `git clone https://github.com/sarcophagus-org/quickstart-archaeologist`


2. Copy example env file, and fill out the ETH_PRIVATE_KEY and PROVIDER_URL fields.

   `cp .env.example .env`


3. Create blank peer ID file.

   `touch peer-id.json`


4. Run the service:

   `docker-compose up`  (or `docker compose up`)

---

### Registering your archaeologist
If this is your first time running the service, you will be prompted to register your archaeologist.

### Further instructions
To start the service at any point after running the initial setup instructions, just use `docker-compose up`

