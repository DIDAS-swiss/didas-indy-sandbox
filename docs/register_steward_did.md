# Register Steward DID

To add a new (validator) node on the pool, the owner has to create a Steward DID. As soon as you (a trustee) get the DID and verkey, you can register it on the ledger.

1. Register DID on leger

    **Change to the machine/vm where you installed the admin indy-cli**

        indy-cli --config ~/cliconfig
        indy> wallet open swiss_sandbox_wallet key
        swiss_sandbox_wallet:indy> did list
        swiss_sandbox_wallet:indy> did use <trustee-did>
        swiss_sandbox_wallet:did(Goo...KxR):indy> pool connect swiss_sandbox
        # use the DID an verkey sent to you from the new steward!
        pool(swiss_sandbox):...:indy> ledger nym did=<DID> verkey=<verkey> role=STEWARD

    You should get something like this:

        Nym request has been sent to Ledger.
        Metadata:
        +------------------------+-----------------+--------------+---------------------+
        | From                   | Sequence Number | Request ID   | Transaction time    |
        +------------------------+-----------------+--------------+---------------------+
        | Gookp43AdBF5KhELhkFKxR | 57              | 166262589... | 2022-09-08 08:30:22 |
        +------------------------+-----------------+--------------+---------------------+
        Data:
        +------------------------+-------------------------+---------+
        | Did                    | Verkey                  | Role    |
        +------------------------+-------------------------+---------+
        | SPRqtyUWSdXj6fuYzxjyZ2 | ~5g9aHRYEvCx4MQ36rweFPm | STEWARD |
        +------------------------+-------------------------+---------+

2. Check DID on Ledger

    At any time you can check if a DID exists on the ledger:

        indy-cli
        indy> pool connect swiss_sandbox
        pool(swiss_sandbox):indy> ledger get-nym did=<DID>