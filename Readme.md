# BUTTCOIN - SNIP-20

This is an adaptation of enigmampc's snip20-reference-impl[https://github.com/enigmampc/snip20-reference-impl].

## Changes made

1. Remove ability to deposit and withdraw scrt tokens.
2. Remove #query_exchange_rate.
3. Remove all references to contract status

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
