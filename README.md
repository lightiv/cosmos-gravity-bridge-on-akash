# cosmos-gravity-bridge-on-akash

This repo is for deploying Cosmos Gravity Bridge (CGB) to Akash Network. Brought to you by _SkyNet | Validators_

## Pre-Requisites

You will need to have a CGB account that is funded.  You will fund the validator and Orchestrator accounts that you are deploying from this account.  You need to download the current binary and create and fund the account.  It can also be downloaded from this repo: https://github.com/lightiv/cosmos-gravity-bridge-on-akash/blob/main/gen_eth_key?raw=true
```
wget https://updates.althea.net/cgb-releases/gravity
chmod +x gravity
sudo mv gravity /usr/bin/gravity

# Create a new key.  Copy the mnemonic and then send fund to this new address
gravity keys add <KEY_NAME>
```

Let's get started.  The first thing we need to do is create and fund our ethereum address

### Step 1: Download ETH Key Generator:
```
mkdir ~/files && cd ~/files
wget https://updates.althea.net/cgb-releases/gen_eth_key
```
