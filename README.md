# cosmos-gravity-bridge-on-akash

This repo is for deploying Cosmos Gravity Bridge (CGB) to Akash Network. Brought to you by _SkyNet | Validators_

## Pre-Requisites

You will need to have a CGB account that is funded.  You will fund the new validator and Orchestrator accounts that you are deploying from this account.  You need to download the current binary and create and fund the account.  It can also be downloaded from this repo: https://github.com/lightiv/cosmos-gravity-bridge-on-akash/blob/main/gen_eth_key?raw=true
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
Run:
```
gen_eth_key
```
This will out put the following.  Save them!:
```
Save this Ethereum Key! Private Key: 0xf3081fe104b72b239xxxxxxxxxxxxxxxxxxxx365b0e8e66668922114e3f39f58 -> Address: 0xFAa0A674xxxxxxxxxxxxxxxxxxxxE2749326E402
```
### Step 2: Send funds to your newly created ETH address
To do this, use on of the methods below to get Geroli ETH.  I always start with the GOERLI FAUCET but many times I fine it not working.  If this happens you can join the Discord RocketPool and use their faucet channel.

**Geroli Faucet:**

https://goerli-faucet.slock.it/

**RocketPool Discord:**

https://discord.gg/ZycVE6K23T

**Use the following Goeli ETH Explorer to verify receipt of funds before continuing:**

https://goerli.etherscan.io/

### Step 3:

Edit your ```deploy.yml``` ```.env``` to add your new ETH address here:
```
- EHEREUM_ADDRESS_FUNDED=
```


