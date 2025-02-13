## Summary
To mine Bitcoin directly into your Electrum wallet using the command line, you can use **NiceHash** or **bitcoin-cli**.

## Explanation
1. **NiceHash**: This platform allows you to mine Bitcoin and get paid directly into your wallet. However, there is a minimum payout threshold, typically **0.001 BTC**, before you can withdraw your earnings. To avoid this, you can use the Lightning Network for instant withdrawals.
2. **bitcoin-cli**: This is the official Bitcoin command line interface. You can use it to interact with the Bitcoin network and manage your wallet. To mine Bitcoin directly into your Electrum wallet, you'll need to configure your mining software to send the mined Bitcoins to your Electrum wallet address.

## Example
Here's a basic example of how to use `bitcoin-cli` to send mined Bitcoins to your Electrum wallet:

```bash
# Start the Bitcoin daemon
bitcoind -daemon

# Wait for the daemon to start
sleep 10

# Get the address of your Electrum wallet
electrum_address=$(electrum getaddresses)

# Send mined Bitcoins to your Electrum wallet
bitcoin-cli sendtoaddress $electrum_address 0.001
```

## References
- [Electrum Documentation](https://electrum.readthedocs.io/en/latest/cmdline.html)
- [NiceHash Support](https://www.reddit.com/r/NiceHash/comments/kzm7h8/any_way_to_mine_directly_into_my_electrum_wallet/)
- [Reddit Discussion on Command Line Tools](https://www.reddit.com/r/TREZOR/comments/j4lp9g/power_of_the_command_line_bitcoincli_hwi_electrum/)

Does this help clarify things for you?
