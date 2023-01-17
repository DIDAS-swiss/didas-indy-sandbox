Adding a node to the ledger means that the ledger is already running and you want to add a new validator node.

First we have to choose an alias for the node. 

- The **node alias** will be a short version of the company running the node, with at least the first letter be uppercase, e.g. HIN or Swisscom

For simplicity reasons we decide to:
- use the default node and client ports 9701 and 9702.
- use only one network interface/ip for the node (not suitable for production!)

These are the steps needed to bootstrap:
- [Order a VM or machine with at least the this requirements](vm_hw_requirements.md)
- [Install a version of the indy commandline tool](install_indy_cli.md)
- [Install git and docker](install_git_and_docker.md)
- [Let node create keys](create_node_keys.md)
- [Generate steward DID and Key](create_steward_did_and_key.md)
- [Let (by a trustee) register steward DID](register_steward_did.md)
- [Connectivity checks](connectivity_checks_phase_1.md)
- [IP filtering](ip_filtering.md)
- [Add node too pool (claim ownership)](add_node_to_pool.md)
- [Start the ledger](start_ledger.md)
- [Connect indy-cli to ledger](connect_indy_cli.md)
