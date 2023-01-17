# Connect indy-cli to ledger (pool)

Now add the DIDAS SSI Sandbox (ledger/pool) to your indy-cli.

Make sure you have the pool_transactions_genesis accessible, then run the following:

(you can download the genesis file from [github][def1])

    indy-cli --config ~/cliconfig
    indy> pool create swiss_sandbox gen_txn_file=<your_path>/pool_transactions_genesis
    indy> pool connect swiss_sandbox
    indy> wallet list
    indy> wallet open swiss_sandbox_wallet key

If you are a trustee you can query the validator info

    indy> did list
    indy> did use <trustee-did-value>
    indy> ledger get-validator-info

[def1]: https://raw.githubusercontent.com/DIDAS-swiss/didas-indy-sandbox/main/genesis.json