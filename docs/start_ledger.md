# Start the ledger

We are ready to start the ledger (for the first time)

So let's start the ledger on each node and see what happens.. I’m pretty sure, it will come up.

So go to the vm and start the ledger.. (initially in the foreground, w/o -d option)

    $ cd ~/indy-node-container/run 
    $ sudo docker compose up

The best will be to have a second shell on the vm..

There we can run:

    sudo docker exec -it indy_node validator-info

When all looks good, lets start the ledger in the background. Type Ctrl-C in the first shell to stop docker-compose (frontend).

    <Ctrl-C>
    sudo docker compose down
    sudo docker compose up -d

From now on you can check the logs with:

    # use -f to stream the logging
    sudo docker logs indy_node -f
    # or
    sudo docker logs indy_node_controller -f

Hint try:

    sudo docker ps