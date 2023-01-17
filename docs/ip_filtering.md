# IP Filtering

<sup>inspired from [How to make iptables configuration persistent using systemd][def]</sup>


Let’s install IP filtering. This can be done in one of two ways (or both). We can install it locally in the VM or in the surrounding hosting (OpenStack or similar). We will do it in the VM, this way we can make it dynamic (later).

We use a so called ipset (kernel table) which allows us to add or remove an IP at any time without fiddling with the iptables itself.

We do the follwoing steps:

1. install a needed package (ipset)
2. create an initial file with the allowed IPs
3. determine the interface
4. create a script to setup ipfilter
5. create a systemd service
6. enable and start service
7. test filtering
8. cross node test

## 1. Install a needed package (ipset)

To install the ipset package run the following command:

    sudo apt install ipset

## 2. Create an ipset with the allowed IPs

Run the following commands to setup the ipset:

    sudo ipset create indy-node-ips hash:ip
    sudo ipset add indy-node-ips <ip-node-1>
    sudo ipset add indy-node-ips <ip-node-2>
    sudo ipset add indy-node-ips <ip-node-3>
    sudo ipset add indy-node-ips <ip-node-4>

## 3. Determine the interface

Determine the name of your network interface (something like ens3)

    ip address list

Look for an interface which is up, not docker or bridge or lo, most probably named ensXX. The output look similar to this one (shortened):

    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
        link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
        inet 127.0.0.1/8 scope host lo
        valid_lft forever preferred_lft forever
        inet6 ::1/128 scope host 
        valid_lft forever preferred_lft forever
    2: ens3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
        link/ether fa:16:3e:af:62:fc brd ff:ff:ff:ff:ff:ff
        inet 10.10.0.5/24 brd 10.10.0.255 scope global dynamic ens3
        valid_lft 499689sec preferred_lft 499689sec
        inet6 fe80::f816:3eff:feaf:62fc/64 scope link 
        valid_lft forever preferred_lft forever
    3: docker0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN group default 
    ...

## 4. Create a script to setup ipfilter

Create a directory where we hold some files related to the indy-node:

    sudo mkdir /etc/indy-node

Put the following code into the file /etc/indy-node/setup-indy-ipfilter.sh (e.g. with sudo vi ..)

**Important: Don’t forget to set the IFACE to the value from above!**

    #!/bin/bash
    # Configure indy-node iptables firewall

    IFACE=ens3

    # Limit PATH
    PATH="/sbin:/usr/sbin:/bin:/usr/bin"

    # iptables configuration
    firewall_start() {
    if test -f /etc/indy-node/ipset.save; then
        ipset restore -f /etc/indy-node/ipset.save
    else
        ipset create indy-node-ips hash:ip
    fi

    # Drop all packets to port 9701 comming not from a known indy node
    iptables -I DOCKER-USER -p tcp --dport 9701 -i $IFACE \
            -m set '!' --match-set indy-node-ips src -j DROP
    iptables -I DOCKER-USER -p tcp --dport 9701 -i $IFACE \
            -m set '!' --match-set indy-node-ips src \
            -j LOG --log-prefix "Indy DROP node-port: "

    # limit concurrent connections 
    iptables -I DOCKER-USER -p tcp --dport 9702 --syn \
            -m connlimit --connlimit-above 16 -j REJECT
    iptables -I DOCKER-USER -p tcp --dport 9702 --syn \
            -m connlimit --connlimit-above 16 \
            -j LOG --log-prefix "Indy REJECT connlimit: "
    }

    # clear iptables configuration
    firewall_stop() {
    iptables -D DOCKER-USER -p tcp --dport 9701 -i $IFACE \
            -m set '!' --match-set indy-node-ips src -j DROP 2> /dev/null
    iptables -D DOCKER-USER -p tcp --dport 9701 -i $IFACE \
            -m set '!' --match-set indy-node-ips src \
            -j LOG --log-prefix "Indy DROP node-port: " 2> /dev/null
    iptables -D DOCKER-USER -p tcp --dport 9702 --syn \
            -m connlimit --connlimit-above 16 -j REJECT 2> /dev/null
    iptables -D DOCKER-USER -p tcp --dport 9702 --syn \
            -m connlimit --connlimit-above 16 \
            -j LOG --log-prefix "Indy REJECT connlimit: " 2> /dev/null
    # save ipset
    if ipset list indy-node-ips 2>&1 > /dev/null; then
        mkdir -p /etc/indy-node
        ipset save indy-node-ips > /etc/indy-node/ipset.save
        ipset destroy indy-node-ips
    fi
    }

    # execute action
    case "$1" in
    start|restart)
        echo "Starting indy ip filter"
        firewall_stop
        firewall_start
        ;;
    stop)
        echo "Stopping indy ip filter"
        firewall_stop
        ;;
    esac


Make it executable:

    sudo chmod 750 /etc/indy-node/setup-indy-ipfilter.sh

Run the script to save the current content of the ipset we created above:

    sudo /etc/indy-node/setup-indy-ipfilter.sh stop

Now there should be a file /etc/indy-node/ipset.save with the following content:

    cat /etc/indy-node/ipset.save
    create indy-node-ips hash:ip family inet hashsize 1024 maxelem 65536
    add indy-node-ips <ip-node-2>
    add indy-node-ips <ip-node-4>
    add indy-node-ips <ip-node-3>
    add indy-node-ips <ip-node-1>

## 5. Create a systemd service

Put the following into the file /etc/systemd/system/indy-node-ipfilter.service (e.g. with sudo vi ..)

    [Unit]
    Description=indy-node ipfilter service
    After=docker.service
    Wants=docker.service

    [Service]
    Type=oneshot
    ExecStart=/etc/indy-node/setup-indy-ipfilter.sh start
    RemainAfterExit=true
    ExecStop=/etc/indy-node/setup-indy-ipfilter.sh stop
    StandardOutput=journal

    [Install]
    WantedBy=multi-user.target

## 6. Enable and start service

To enable (start at boot) ans start (immediate) run the command:

    sudo systemctl daemon-reload
    sudo systemctl enable indy-node-ipfilter
    sudo systemctl start indy-node-ipfilter
    sudo systemctl status indy-node-ipfilter

Now let’s check the iptables rules:

    sudo iptables -L DOCKER-USER

The output should look like the following:

![](./img/docker-user-table.png)

You see that we added both rules twice, one to log, the other to do the action (DROP, REJECT).

## 7. Test filtering

Test the filtering. As we already started the ipfilter, it should no lomger be possible to connect to port 9701 from a random machine on the internet. To try this out run the follwoing command which spins up a docker container with a net-cat programm. It is necessary to run it inside docker, because if we would run it just in the local shell, the filter would not trigger. (Do you know why?) It’s because ip traffic to the vm itself gets routed through the INPUT chain, but traffic to tge docker container gets routed through the FORWARD chain. And thats where our rules are.

    sudo docker run --rm -it -p 9701:9701 busybox nc -v -lk -p 9701 -e true

Now run on any internet connected machine the follwing:

    nc -vz <your-nodes-ip> 9701

It should NOT be able to connect! Stop both command with Ctrl-C.

Now lets check the log entries:

    grep Indy /var/log/kern.log

You should see some log entries from your (hopefully) failes attemtps to connect.

As a lst test, we rebbot the vm and check that the ipset and rules are loaded:

    sudo reboot
    # wait till rebooted
    # login again
    sudo ipset list indy-node-ips
    sudo iptables -L DOCKER-USER

You should see the 4 IPs and then the 5 rules.

## 8. Cross node test

As a final test each one starts the following command on the node and in a second shell (second login) we try to connect to each other node ip:

    # in the first shell
    sudo docker run --rm -it -p 9701:9701 busybox nc -v -lk -p 9701 -e true

**Please send here an email to the other node admins when the above command is running**

    # in the second shell
    nc -vz <ip-node-3> 9701
    nc -vz <ip-node-4> 9701
    nc -vz <ip-node-1> 9701
    nc -vz <ip-node-2> 9701

When you see in the first shell a connection in the output for each of the other nodes, you can stop the command (Ctrl-C) and close the second shell (Ctrl-D)

[def]: https://sleeplessbeastie.eu/2018/10/01/how-to-make-iptables-configuration-persistent-using-systemd/