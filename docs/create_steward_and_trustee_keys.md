# Generate Stewart and Trustee DID and Key

<sup>inspired from: [Validator Preparation Guide - Indicio][def1]</sup>

**Precondition: indy-cli ist installed**

indy-cli is the command-line tool to manage local wallets and remote indy-nodes.

## 1. Add an Acceptance Mechanism

To write to the Swiss-Sandbox Ledger, (in the future) you’ll need to sign the Transaction Author Agreement. This Agreement is incorporated into the process of connecting to the node pool and requires an acceptance mechanism. For the Indy CLI, the default mechanism is “For Session” and the following instructions are required to be able to use “For Session” for your CLI:

Create a JSON Config file containing your taaAcceptanceMechanism. (You can also add plugins to this config file, but for now just set it up as basic as possible.)

    vi ~/cliconfig

This example cliconfig file contains the line that sets the AML:

    {
        "taaAcceptanceMechanism": "for_session" 
    }

To start the indy-cli using your new config file, run the following:

    indy-cli --config ~/cliconfig
    "for_session" is used as transaction author agreement acceptance mechanism
    indy> exit
    Goodbye...

Now all of the appropriate transactions will have an “Agreement Accepted” authorization attached to them during this CLI session.

## 2. Generate the Steward and Trustee Seeds

Next, generate a Steward and Trustee key using the CLI machine you just installed. This will be comprised of a public and private key pair, generated from a seed. Knowing your seed will allow you to regenerate the key on demand. To keep this secure, you will need to have a very secure seed that is not easy to guess. 

Generate two Seeds

**Important: You want to guard your seed well. The seed will be used to generate your public (verification) key as well as your secret private key. If your seed falls into the wrong hands, someone could regenerate your private key, and take over your identity on the ledger. Keys can be rotated, which can stop some of the damage, but damage will still have been done.**

In the terminal, run the following to install a good random string generator, and then use it to generate two seeds: 

    sudo apt install pwgen
    pwgen -s 32 2 -1

EXAMPLE (generate your own!):

    pwgen -s 32 2 -1
    c4URDlPXFStv5cbQnCfsHABCOMWAhEU3
    GG0UKcXsGvklNa0QY9tlp7FxLrluG9WZ

**Important: Keep this seeds in a safe place, such as an encrypted password manager or other secure location designated by your organization. You will need it later in this guide, as well as in the future for other Node Operator interactions with the ledger.**

## 3. Create a local Wallet

Next we run the indy-cli command line CLI by entering: 

    indy-cli --config ~/cliconfig

In the command line, enter the following to create our wallet locally. In these instructions, we use **"swiss_sandbox_wallet"** for the wallet name, although you may use an other name of your choosing, if desired.

The encrypted wallet will be used to store important information on this machine, such as your public and private keys. When creating your wallet, you will need to provide a **"key"** that is any string desired. It will be the encryption key of your local wallet. You have to enter this key everytime you open the wallet in the indy-cli.

    indy> wallet create swiss_sandbox_wallet key

Upon entering this command, you’ll see a prompt to enter your wallet key. Enter the key and hit return.

    Enter value for key:

    Wallet "swiss_sandbox_wallet" has been created
    indy> 

**Important: To be able to retain your wallet and not re-create it when you need it in the future, keep this wallet key in a secure location as well.**

## 4. Generate the Steward and Trustee keys and DID

    indy> wallet open swiss_sandbox_wallet key

*-> enter the key you have choosen before*

Using the seeds that you generated with pwgen, place your public and private keys into your wallet.

Make sure you note with your generated seed, which one you use for the Steward and which one for the Trustee DID/Key. We start here with the Steward, so the first one ist the Steward-Seed.

    swiss_sandbox_wallet:indy> did new seed metadata="steward did"

*-> enter the first seed*

This should look like this:

    Enter value for seed:

    Did "QknDW12a3QzpEezWrw5eBi" has been created with "~PR9NyXkMnPD3FTJB5tNqnn" verkey
    Metadata has been saved for DID "QknDW12a3QzpEezWrw5eBi"
    swiss_sandbox_wallet:indy> 

**Important: Save the “DID”  and “verkey” portions of this. They are not secret, and they will be used when you are prompted to supply your Steward verkey and DID.**

    swiss_sandbox_wallet:indy> did new seed metadata="trustee did"

*-> enter the second seed*

**Important: Save the “DID”  and “verkey” portions of this. They are not secret, and they will be used when you are prompted to supply your Trustee verkey and DID.*

Close the wallet

    swiss_sandbox_wallet:indy> exit

[def1]: https://docs.google.com/document/d/1y0rW78_I-bRkH3qFN5kcP58jJH23Zahc363h18SmJ48/edit#