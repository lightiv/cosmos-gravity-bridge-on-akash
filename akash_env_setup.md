Go here to get the akash binary: [Akash Binary Releases](https://github.com/ovrclk/akash)

See Releases on the right side of the page.  

# Setup Environment:
```
AKASH_NODE=http://135.181.60.250:26657
AKASH_CHAIN_ID=akashnet-2
AKASH_ACCOUNT_ADDRESS=<FUNDED_AKASH_ADDRESS>
AKASH_KEYRING_BACKEND=os
AKASH_KEY_NAME=skynet-tehp9
AKASH_OSEQ=1
AKASH_GSEQ=1
```
## Verify Settings:
```
echo $AKASH_NODE $AKASH_CHAIN_ID $AKASH_KEYRING_BACKEND
```
```
Output:
http://135.181.60.250:26657/ akashnet-2 os
```
```
echo $AKASH_KEY_NAME
```
```
Output:
<YOUR_KEY_NAME>
```
```
export AKASH_ACCOUNT_ADDRESS="$(akash keys show $AKASH_KEY_NAME -a)"

echo $AKASH_ACCOUNT_ADDRESS

Output:
akash1j8s87w3fctzxxxxxxxxxxxxxxxxxxxx0443eksx
```

## Check your account has sufficient balance by running:
```
akash query bank balances --node $AKASH_NODE $AKASH_ACCOUNT_ADDRESS

Output:
balances:
- amount: "93000637"
  denom: uakt
pagination:
  next_key: null
  total: "0"
```
You will also need to create an Akash deployment certificate to be able to deploy to the Akash DeCloud.  Execute the following on your deployment node:

**:warning: certificate needs to be created only once per account and can be used across all deployments.**

```
akash tx cert create client --chain-id $AKASH_CHAIN_ID --keyring-backend $AKASH_KEYRING_BACKEND --from $AKASH_KEY_NAME --node $AKASH_NODE --fees 10000uakt
```
