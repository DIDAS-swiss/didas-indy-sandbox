# Add new IP to pool

To add a new node to the ledger pool, it is necessary to also add the respective IP to the iptables table. The following code (executed on the node’s vm) adds the ip to the table and saves the extended table.

    sudo bash
    ipset add indy-node-ips <new-nodes-ip>
    ipset save indy-node-ips > /etc/indy-node/ipset.save
    exit