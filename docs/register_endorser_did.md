# Register Endorser DID

If you plan to implement any use case you need an Endorser-DID (or have access to an endorser). The easiest way is to be your own endorser, means you are both issuer and endorser with the same DID.

To register an (endorser) DID on the ledger follow the stepts below.

1. Create a (endorser) DID

    As in previouse capters we use indy-cli to crete a DID. But first we have to crete a new Seed for ths DID.

        pwgen -s 32 1

    **Important: Keep this seeds in a safe place, such as an encrypted password manager or other secure location designated by your organization. You will need it later in this guide.**

    If you start with a new Use-Case you probaly have to install  indy-cli first. Don’t use the instance you created for node admin (even if it is possible). If you do it anyway, at least create a new wallet.

    Following I assume you installed indy-cli and have created a wallet named “usecase1”.

    Now let the wallet create a DID from the Seed. (You can delete the wallet if you like, currently we only need the DID and verkey).

        indy-cli
        indy> wallet open usecase1 key
        usecase1 indy> did new seed
        usecase1 indy> exit

    You see now (in green color) the created DID and verkey. In the next step we register them as endorser in the ledger.

2. Register DID on leger (as trustee)

    If you are not a trustee you have to let a trustee do this step

    **Now change to the machine/vm where you installed the admin indy-cli**

        indy-cli --config ~/cliconfig
        indy> wallet open swiss_sandbox_wallet key
        swiss_sandbox_wallet:indy> did list
        swiss_sandbox_wallet:indy> did use <trustee-did>
        swiss_sandbox_wallet:did(Goo...KxR):indy> pool connect swiss_sandbox
        # now use the DID an verkey created above!
        pool(swiss_sandbox):...:indy> ledger nym did=<DID< verkey=<verkey> role=ENDORSER

3. Check DID on Ledger

    At any time you can check if a DID exists on the ledger:

        indy-cli
        indy> pool connect swiss_sandbox
        pool(swiss_sandbox):indy> ledger get-nym did=7qAiBRKn6CZeDbf69wJJX4

    You should get the following:

        Following NYM has been received.
        Metadata:
        +------------------------+-----------------+---------------------+---------------------+
        | Identifier             | Sequence Number | Request ID          | Transaction time    |
        +------------------------+-----------------+---------------------+---------------------+
        | LibindyDid111111111111 | 9               | 1655113874014895000 | 2022-06-12 13:51:09 |
        +------------------------+-----------------+---------------------+---------------------+
        Data:
        +------------------------+------------------------+-------------------------+----------+
        | Identifier             | Dest                   | Verkey                  | Role     |
        +------------------------+------------------------+-------------------------+----------+
        | Gookp43AdBF5KhELhkFKxR | 7qAiBRKn6CZeDbf69wJJX4 | ~GZKv5gVLHjYbXXxyAkfj8e | ENDORSER |
        +------------------------+------------------------+-------------------------+----------+