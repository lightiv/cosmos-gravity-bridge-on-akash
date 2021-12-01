# Setup Environment:
```
AKASH_NODE=http://135.181.60.250:26657
AKASH_CHAIN_ID=akashnet-2
AKASH_ACCOUNT_ADDRESS=akash14s4tlmlvy0vsps05csf8l80m7nqxmau38tehp9
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
akash1j8s87w3fctz7nlcqtkl5clnc805r240443eksx
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
