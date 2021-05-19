# Buttcoin (BUTT)
The native token of [btn.group](https://www.btn.group) built on the Secret network blockchain.

## Changes made

1. Remove ability to deposit and withdraw scrt tokens.
2. Remove #query_exchange_rate.
3. Remove all references to contract status
4. Remove ability to make total supply private.
5. Remove ability to set initial balances.
6. Remove ability to burn tokens.
7. Add block_height to transaction(Tx)
  * Decided not to store block.time as this can be obtained via the chain and want to keep the contract as lean as possible.
8. Store minting in transfer history.

## Local code examples
```
INIT='{"name": "buttcoin", "symbol": "BUTT", "decimals": 12, "prng_seed": "YXNkZmFzZGZhc2RmYXNkZnNhZGZhc2RmYXNkZg=="}'
CODE_ID=1
secretcli tx compute instantiate $CODE_ID "$INIT" --from a --label "buttcoin" -y --keyring-backend test
CONTRACT={ address of contract instantiated }
secretcli query compute query $CONTRACT '{"token_info": {}}'
# Code below for setting minters probably needs tweaking.
secretcli tx compute execute $CONTRACT '{"set_minters": { "minters": "[secret000000000000000000000000FRIED]" }}' --from a --keyring-backend test
```

## TO DO - ADAPT THIS TO THIS README

https://github.com/enigmampc/SecretSwap/blob/master/contracts/secretswap_token/README.md

## Verifying build

Given the address of a contract, you can query its code hash (sha256) by running:
```
secretcli q compute contract-hash <contract-address>
```

You can verify that this hash is correct by comparing it to the decompressed
contract binary.

To get the contract binary for a specific tag or commit and calculate its hash,
run:
```
git checkout <tag-or-commit>
make compile-optimized-reproducible
gunzip -c contract.wasm.gz >contract.wasm
sha256sum contract.wasm
```

Now compare the result with the hash returned by `secretcli`.
If you compiled the same code that was used to build the deployed binary,
they should match :)

## References

1. https://github.com/enigmampc/snip20-reference-impl
2. https://github.com/SecretFoundation/SNIPs/blob/master/SNIP-20.md
3. https://github.com/enigmampc/scrt-finance-rewards/tree/master/contracts/gov-token
4. https://github.com/enigmampc/SecretSwap/blob/master/contracts/secretswap_token/README.md
