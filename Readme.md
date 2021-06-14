# Buttcoin (BUTT)
The native token of [btn.group](https://btn.group) built on the Secret network blockchain.

## Example contracts
* Holodeck testnet: [secret1a75javymnp05egne7mq008rw3s6m3yglfaggqt](https://secretnodes.com/secret/chains/holodeck-2/contracts/secret1a75javymnp05egne7mq008rw3s6m3yglfaggqt)
* Production: [secret1yxcexylwyxlq58umhgsjgstgcg2a0ytfy4d9lt](https://secretnodes.com/secret/chains/secret-2/contracts/secret1yxcexylwyxlq58umhgsjgstgcg2a0ytfy4d9lt)

## Changes made to standard SNIP-20/SNIP-21 implementations 
1. Remove ability to deposit and redeem scrt tokens.
2. Remove ability to change contract status.
3. Remove ability to make total supply private.
4. Remove ability to mint and burn tokens.
5. Remove all reference to admin.

## Current limitations/recommendations as per review by [baedrik](https://github.com/baedrik)
1. Just an FYI, while working on a contract that builds off of snip22, I noticed a couple small bugs. One of them is in the logging of Burn transactions, so it doesn't affect your token, but one that does is in store_transfer in transaction_history.rs:
```
    if owner != sender {
        append_tx(store, &tx, owner)?;
        append_transfer(store, &transfer, owner)?;
    }
    append_tx(store, &tx, sender)?;
    append_transfer(store, &transfer, sender)?;
    append_tx(store, &tx, receiver)?;
    append_transfer(store, &transfer, receiver)?;
```
Should be changed to:
```
    if owner != sender && owner != receiver {
        cosmwasm_std::debug_print("saving transaction history for owner");
        append_tx(store, &tx, owner)?;
        append_transfer(store, &transfer, owner)?;
    }
    if sender != receiver {
        cosmwasm_std::debug_print("saving transaction history for sender");
        append_tx(store, &tx, sender)?;
        append_transfer(store, &transfer, sender)?;
    }
    cosmwasm_std::debug_print("saving transaction history for receiver");
    append_tx(store, &tx, receiver)?;
    append_transfer(store, &transfer, receiver)?;
```
If a contract uses an allowance to send itself some tokens, the tx will appear twice in its history. Or if the sender and receiver address are the same (which snip22 does not reject) that will appear twice as well. Neither of those will happen frequently, but still best to have clean code.

## References
1. https://github.com/enigmampc/snip20-reference-impl
2. https://github.com/SecretFoundation/SNIPs
3. https://github.com/enigmampc/scrt-finance-rewards/tree/master/contracts/gov-token
4. https://github.com/enigmampc/SecretSwap/blob/master/contracts/secretswap_token/README.md
