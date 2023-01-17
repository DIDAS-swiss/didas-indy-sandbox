# Install indy-cli

<sup>Good source: https://indicio.tech/selfserve/ (Appendix A: Installing the Indy CLI)</sup>

We will use `indy-cli` to interact with the node. It should **NOT** be installed on the node itself, but on another vm/server/laptop. There you will create and store your steward and trustee keys.


The setup of an Indy-CLI can done in three different ways.

- a) Installing it locally
- b) Running in Docker
- c) Installation on a server

---

## a) Installing it locally

Follow the instructions for your OS (Windows, Mac, Linux): [indy-sdk/cli][def1]

---

## b) Running in Docker

A fast and easy way to use the docker image from British Columbia.

    docker run -it bcgovimages/von-image:py36-1.15-0 indy-cli

If files like the pool_transaction_file have to be reached within the *indy-cli* container, they can be accessed under /home/indy/path-to-file.

---

## c) Installation on a server/laptop

The *indy-cli* can also be installed on a server. This provides a more persistent and centralized solution for accessing a ledger.

<sup>For more information about installing an Indy-CLI see: [Sovrin Steward 
Validator Preparation Guide](def2)</sup> 

On the server execute the following instructions.
```bash
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys CE7709D068DB5E88 
sudo apt-get install -y software-properties-common python-software-properties 
sudo add-apt-repository "deb https://repo.sovrin.org/sdk/deb  xenial stable"
sudo add-apt-repository "deb https://repo.sovrin.org/deb  xenial stable"

sudo apt-get update -y
sudo apt-get upgrade -y
sudo apt-get install -y indy-cli
````

[def1]: https://github.com/hyperledger/indy-sdk/tree/master/cli
[def2]: https://docs.google.com/document/d18MNB7nEKerlcyZKof5AvGMy0GP9T82c4SWaxZkPzya4/edit#