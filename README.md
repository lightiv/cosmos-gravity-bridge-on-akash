# cosmos-gravity-bridge-on-akash 

** **THIS IS THE FINAL TESTNET VERSION** **

This repo is for deploying Cosmos Gravity Bridge (CGB) to Akash Network. Brought to you by _SkyNet | Validators_

#### Credit:  This project is a derivative of [Akash Network Cosmos Omnibus](https://github.com/ovrclk/cosmos-omnibus) 

## Pre-Requisites

You need a Linux node with the current Akash binary configured in your path. Which you can get here: 

[Akash Network Environment Setup](https://github.com/lightiv/cosmos-gravity-bridge-on-akash/blob/main/akash_env_setup.md)

You will need to have a CGB account that is funded.  You will fund the new validator and Orchestrator accounts that you are deploying from this account.  You need to download the current binary and create and fund this account.  It can be downloaded from this repo: https://github.com/lightiv/cosmos-gravity-bridge-on-akash/blob/main/gen_eth_key?raw=true

### Private key backup/restore via Filebase

On boot, the container can restore a private key from any S3 storage provider. If this is configured and the key doesn't exist in S3 yet, it will be uploaded from the node.

This allows for a persistent node ID and validator key on Akash's ephemeral storage.

[Filebase](https://filebase.com/) is a great way to use S3 with decentralised hosting such as Sia or Storj.

The `node_key.json` and `priv_validator_key.json` are both backed up, and can be encrypted with a password.

|Variable|Description|Default|Examples|
|---|---|---|---|
|`S3_KEY`|S3 access key| | |
|`S3_SECRET`|S3 secret key| | |
|`S3_HOST`|The S3 API host|`https://s3.filebase.com`|`s3.us-east-1.amazonaws.com`|
|`KEY_PATH`|Bucket and directory to backup/restore to| |`bucket/nodes/node_1`|
|`KEY_PASSWORD`|An optional password to encrypt your private keys. Shouldn't be optional| | |

You also need to have a filebase account for backup of your keys

Private keys can be backed up and restored from any S3 compatible storage provider, such as Sia or Storj via Filebase.

Let's get started.  The first thing we need to do is create and fund our ethereum address

### Step 1: Download ETH Key Generator:
```
mkdir ~/cgb-files && cd ~/cgb-files
wget https://updates.althea.net/cgb-releases/gen_eth_key && chmod +x gen_eth_key
```
Run:
```
./gen_eth_key
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

### Step 3: Edit your ```deploy.yml``` ```.env``` to add your new ETH privae key and address here:
```
- EHEREUM_ADDRESS_FUNDED=
- ETHEREUM_PRIVATE_KEY=
```

### Step 4: Finish editing the ```deploy.yml``` ```.env``` to 
```
    env:
      - MONIKER=<VALIDATOR_NAME>
      - CHAIN_BINARY=https://cgb.s3.filebase.com/gravity
      - GENESIS_URL=https://cgb.s3.filebase.com/genesis.json
      - CHAIN_ID=cgbtestnet1
      - P2P_SEEDS=
      - P2P_PERSISTENT_PEERS=199b185604cac73ee794cd4ab226d4f26d610ea1@144.202.92.109:26656,0420b3434b4e31aa12257b9c3e4bf93fe5f86e21@chainripper-2.althea.net:26656,c4e8b828fec1121015dfabbb2b>
      - STATESYNC_RPC_SERVERS=
      - VALIDATOR_KEY_NAME=
      - ORCHESTRATOR_KEY_NAME=
      - NODE_TYPE=validator
      - NODE_P2P_PEX=false
      - NODE_UNCONDITIONAL_PEER_IDS=
      - NODE_ADDR_BOOK_STRICT=false
      - NODE_DOUBLE_SIGN_CHECK_HEIGHT=10
      - ETHEREUM_RPC_NODE=http://testnet2.althea.net:8545
      - ETHEREUM_PRIVATE_KEY=
      - ETHEREUM_CONTRACT_ADDRESS=0xace45Cd2d490a0A180e50144D8dd0c7EB9A4215f
      - VALIDATOR_BOND_AMOUNT=1000000ugraviton
      - VALIDATOR_COMMISSION_RATE=0.10
      - VALIDATOR_MAX_COMMISSION_RATE=0.20
      - VALIDATOR_MAX_COMMISSION_CHANGE_RATE=0.01
      - VALIDATOR_MIN_SELF_DELEGATION=1
      - VALIDATOR_FEES=200ugraviton
      - VALIDATOR_DETAILS="<VALIDATOR_DESCRIPTION>"
      - VALIDATOR_SECURITY_CONTACT=
      - VALIDATOR_WEBSIE=https://skynetvalidators.com
      - VALIDATOR_IDENTITY=<KEYBASE_ID>
      - ORCHESTRATOR_FEES=1ufootoken
      - REMOTE_RPC_NODE=<A_CGB_PUBLIC_RPC_NODE>
      - S3_KEY=
      - S3_SECRET=
      - S3_HOST=https://s3.filebase.com
      - KEY_PATH=
      - KEY_PASSWORD=
      - MINIMUM_GAS_PRICES=
```
Edit the above variables as necessary
 
### Step 5: Deploy Cosmos Gravity Bridge

From this point forward please do not take too long to finish the steps.  If you take too long the bid will close and you will need to close the deployment and start over from this step.  The bid will close in 1 to 2 minutes after the next step.
 
#### Create a the deployment for Providers to bid on
```
akash tx deployment create ./deploy.yml --from $AKASH_KEY_NAME --node $AKASH_NODE --chain-id $AKASH_CHAIN_ID --fees 12000uakt -y
```
From the output fine the DSEQ number and add it to the following variable:
```
AKASH_DSEQ=<XXXXXX>
```
Verify the correct varibles:
```
echo $AKASH_DSEQ $AKASH_OSEQ $AKASH_GSEQ

output:
140324 1 1
```
#### View Bids:
```
akash query market bid list --owner $AKASH_ACCOUNT_ADDRESS --node $AKASH_NODE --dseq $AKASH_DSEQ -o text 
```
#### Select a provider from the listed BIDs
From the prior command, select a provider and add their address to the following variable:
```
AKASH_PROVIDER=<PROVIDER_ADDRESS>
```
Verify Setting:
```
echo $AKASH_PROVIDER

Output:
<PROVIDER_ADDRESS>
```
#### Create Your Lease:
```
akash tx market lease create --from $AKASH_KEY_NAME --fees 10000uakt --gas auto  --owner $AKASH_ACCOUNT_ADDRESS --dseq $AKASH_DSEQ --gseq $AKASH_GSEQ --oseq $AKASH_OSEQ --provider $AKASH_PROVIDER --chain-id $AKASH_CHAIN_ID --node $AKASH_NODE 

output:
{"height":"565412","txhash":"01019A27B6CF1064XXXXXXXXXXXXXXXXXXXXCB78C07A79049F66E2AE8E336681","codespace":"","code":0,"data":"0A0XXXXXXXXXXXXXXXXXXXXC65617365","raw_log":"[{\"events\":[{\"type\":\"akash.v1\",\"attributes\":[{\"key\":\"module\",\"value\":\...
```
#### Upload Manifest:
```
akash provider send-manifest ./deploy.yml --node $AKASH_NODE --dseq $AKASH_DSEQ --provider $AKASH_PROVIDER --home ~/.akash --from $AKASH_KEY_NAME
```
there will be no output

#### View Lease:
```
akash query market lease list --owner $AKASH_ACCOUNT_ADDRESS --node $AKASH_NODE --dseq $AKASH_DSEQ -o text
```
Retrieve the access details by running the below:
```
akash provider lease-status --node $AKASH_NODE --home ~/.akash --dseq $AKASH_DSEQ --from $AKASH_KEY_NAME --provider $AKASH_PROVIDER
```

The following command will be utilized to view your newly created addresses and mnemonics.
```
akash provider lease-logs --from $AKASH_KEY_NAME --dseq $AKASH_DSEQ --gseq $AKASH_GSEQ --oseq $AKASH_OSEQ --provider $AKASH_PROVIDER --node $AKASH_NODE > plogs
```
With the above command you it will create the file ```plogs``` which will conatain your new addresses and mnemonics.  Use ```nano``` or ```vi``` to view the file.

