# Upgrading the ledger

To upgrade the ledger we use the automated upgrade via a transaction initiated via *indy-cli*.

The upgrade transaction can currently be initiated by any trustee.

Start indy-cli and open the wallet and connect to the ledger.

    indy-cli --config ~/cliconfig
    indy> pool connect swiss_sandbox
    indy> wallet open swiss_sandbox_wallet key

The following command is an example from an upgrade end of 2022 to indy node 1.12.6. You have to adapt the command accordingly.

    ledger pool-upgrade name=DIDAS-20221228 action=start force=true package=indy-node \
      reinstall=true \
      sha256=f0e7f5f5d457676942c92dd58c54226159870d417dc46490ad44a6d3b9ff7238 \
      version=1.12.6 schedule={"YsS3g5bXytDafKmV2obSysv12cLNMqEDS4M8rgDcH9A":"2022-12-28T10:30:00.258870+01","DJAP6NxxRWNmtBMdpk3o2QR7GskvJcfnsy3KPXuuyo6P":"2022-12-28T10:30:00.258870+01","AuA4G3TrJhHzGCF6DnySkLTYr7sfu3WghJrMSy3FkLnA":"2022-12-28T10:30:00.258870+01","ETEqKy5CmTLShdak9WGFXeTaa1w6jm8ohjjn1YaAEZv3":"2022-12-28T10:30:00.258870+01","G5D2q7DQj5U2gXsvft3cUZ9Z76TEP8uYGZhebUcXjqKt":"2022-12-28T10:30:00.258870+01","7qiGBhbraNrccJ7GM8kVqwfjiHNy8sNGP8UqvFrrbGp3":"2022-12-28T10:30:00.258870+01"}

To browse the transactions in the ledger there is an [IndyScan](https://indyscan.sb.didas.swiss) instance running.
