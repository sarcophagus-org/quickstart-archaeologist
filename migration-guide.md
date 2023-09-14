### BEFORE updating your docker containers:

- `COMPOSE_PROFILES=service docker compose down --remove-orphans`
- `COMPOSE_PROFILES=service docker compose stop`

### Update your provider url

The archaeologist service has been updated to use a websocket provider.
Your provider url will need to be updated to continue running. It should now be in the `wss:://` format.
For example, for infura mainnet, it would look like this: `wss://mainnet.infura.io/ws/v3/<your-infura-api-key>`

### Update your node environment by running these commands in order:

- `docker image prune`
- `git pull`
- `COMPOSE_PROFILES=all-networks docker compose pull`
- `docker compose up archaeologist -d`

The quickstart repository readme has been updated to include instructions on running on multiple networks.
See the heading "Running on multiple networks".

**Note:**

If you already have a node running on or registered on a testnet, you may see a warning in the logs (`docker logs <container-id> --since 10m`) about your profile not matching what is stored in the contracts.

You will need to update your contracts profile to resolve this:

- `docker compose exec -it archaeologist-goerli sh` (or `docker compose exec -it archaeologist-sepolia sh` for sepolia)
- `cli update -u`
- `exit`

### Setting up notifications:

The quickstart repository readme has been updated to include instructions on setting up notificatoins.
See the heading "Notifications".
