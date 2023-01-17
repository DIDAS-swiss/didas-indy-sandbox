# Connectivity Checks - Phase I

To be sure everything is setup correctly so far, we run some connectivity checks. The local ipfilter is not yet setup, so connections from any machine to the node-2-node port is still possible.

## 1. Check IP and node port

In this first step we check the node-port. On the node run the following command and wait for a connection:

    nc -l 9701

Remark: we don’t specify an IP here, because currently we only have one interface.

Now run the following command on any machine on the internet to check the node (inbound) connectivity:

    nc -vz <node_ip_address> 9701

If everything is ok then the upper command on the node, just terminates without any output.
The other command prints out something like:

    Connection to <ip> port 9701 [tcp/*] succeeded!

## 2. Repeate this same procedure for the client port 9702