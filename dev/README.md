# Purpose:

This is a generic repository that just assembles other pieces into a more dev-friendly ecosystem where one may run all the essential infrastructure needed for TDA.

# Prerequisites:
- [von-network](https://github.com/bcgov/von-network)
- [docker](https://docs.docker.com/engine/install)
- [docker-compose](https://docs.docker.com/compose/install/)

# Docker + Docker-compose quick install guide

On Mac and Windows, Docker and Docker Compose are included in "Docker Desktop" install.
On Ubuntu these commands worked for me(these probably do not provide the latest version):

```
sudo apt install docker.io
sudo systemctl enable docker
sudo systemctl start docker
sudo systemctl status docker

sudo apt install docker-compose
```

# Von-network quick install 

These are for Ubuntu / Linux, if you are using something else you will probably need to adapt the commands but the idea should be the same:

```
git clone https://github.com/bcgov/von-network.git
cd von-network
sudo ./manage build
```


# Running up
1. Clone the repository `git clone https://github.com/THCLab/tda-dev-env.git`
1. Go to von-vetwork dir and run it with `./manage start`
1. In tda-dev-env dir run `docker-compose up`. It serves:
    * [toolbox.localhost](https://github.com/THCLab/aries-toolbox)
    * [repository.localhost](https://github.com/THCLab/odca-search-engine)
    * [data-vault.localhost](https://github.com/THCLab/oca-data-vault)
    * [agent1.localhost](https://github.com/THCLab/aries-cloudagent-python)
    * agent1-admin.localhost
    * [agent2.localhost](https://github.com/THCLab/aries-cloudagent-python)
    * agent2-admin.localhost

# Scenario
### Issue Credential with filled OCA form:
1. [Run von-network and aries ecosystem](#running-up)
1. Upload schema to OCA Repository
   ```
   curl -F 'file=@oca-schema.zip' repository.localhost/api/v2/schemas/test
   ```
1. Open `toolbox.localhost` in two browser windows
1. Get agents' invitation urls with `docker logs aries_agent1.localhost_1` and `docker logs aries_agent2.localhost_1`. Copy them to toolboxes - one per toolbox. Then connect into agents.
1. Connect agents together
    * In Main agent: go to __Invitations__, create new invitation and copy URL
    * In Client agent: go to __Connections__, paste invitiation url and add it (click Refresh button to see result)
1. In Main agent go to DIDs module, publish selected DID in the ledger and activate it
1. Issue credential
    * In Main agent: go to __Credential Issuance__
    * Search for `HashedStructure` schema and click `Issue`
    * Select `Client` connection and go next
    * Fill all schema fields and go next
    * Send credential
1. In Client agent check credential in __My Credentials__
