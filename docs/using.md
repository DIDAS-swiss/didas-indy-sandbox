# Using the ledger

If you wanna issuer or verify the ledger you will use the ledger.

In both cases the first tghing you have to do is to connect your indy-cli to the ledger (or otherwise configure your tools for the ledger).

The genesis file is available in [github][def1].

- [Connect indy-cli to ledger](connect_indy_cli.md)

If you want to issuer credentials you have to be an endorser (or use an other endorser). To get an endorser you have to create and let register an endorser DID.

- [Register endorser DID](register_endorser_did.md)

To browse the transactions in the ledger there is an [IndyScan](https://indyscan.sb.didas.swiss) instance running.

[def1]: https://raw.githubusercontent.com/DIDAS-swiss/didas-indy-sandbox/main/genesis.json