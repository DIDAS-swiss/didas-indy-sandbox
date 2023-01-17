# Bootstrapping the Ledger

Bootstrapping the ledger means setting up the first (at least) four ledger nodes. During this process the so called *genesis* file gets created. This file will then be used to connect to the ledger by any further node or client (wallet).

First we have to choose an alias for each node and trustee. 

- The **node alias** will be a short version of the company running the node, with at least the first letter be uppercase, e.g. HIN or Swisscom
- The **trustee alias** will then be simply the all lowecase version of the node alias, e.g. hin or swisscom

For simplicity reasons we decide to:
- use the default node and client ports 9701 and 9702.
- use only one network interface/ip for the node (not suitable for production!)

These are the steps needed to bootstrap:
- [Order a VM or machine with at least the this requirements](vm_hw_requirements.md)
- [Install a version of the indy commandline tool](install_indy_cli.md)
- [Install git and docker](install_git_and_docker.md)
- [Let node create keys](create_node_keys.md)
- [Generate Stewart and Trustee DID and Key](create_steward_and_trustee_keys.md)
- [Connectivity Checks](connectivity_checks_phase_1.md)
- [IP filtering](ip_filtering.md)
- [Generate genesis files](generate_genesis_files.md)
- [Start the ledger](start_ledger.md)
- [Connect indy-cli to ledger](connect_indy_cli.md)
