## Summary
To mine Bitcoin directly into your Electrum wallet using the command line, you can use **NiceHash** or **bitcoin-cli**.

## Explanation
1. **NiceHash**: This platform allows you to sell your mining power and get paid in Bitcoin. However, you need to reach a minimum payout threshold before you can withdraw your earnings. Once you reach the threshold, you can transfer the Bitcoin to your Electrum wallet.
2. **bitcoin-cli**: This is the command-line interface for Bitcoin Core. You can use it to mine Bitcoin and manage your wallet. To mine directly into your Electrum wallet, you need to import your Electrum wallet's private key into Bitcoin Core.

## Example
Here's an example of how to use `bitcoin-cli` to mine Bitcoin and transfer it to your Electrum wallet:

```bash
# Start Bitcoin Core daemon
bitcoind -daemon

# Import Electrum wallet's private key
bitcoin-cli importprivkey <your_private_key> <your_wallet_password>

# Start mining
bitcoin-cli getblocktemplate

# Send mined Bitcoin to your Electrum wallet
bitcoin-cli sendtoaddress <your_electrum_wallet_address> <amount>
```

## References
- [NiceHash](https://www.reddit.com/r/NiceHash/comments/kzm7h8/any_way_to_mine_directly_into_my_electrum_wallet/)
- [Electrum Documentation](https://electrum.readthedocs.io/en/latest/cmdline.html)
- [Bitcoin Core CLI](https://www.reddit.com/r/TREZOR/comments/j4lp9g/power_of_the_command_line_bitcoincli_hwi_electrum/)

Does this help clarify things for you?
