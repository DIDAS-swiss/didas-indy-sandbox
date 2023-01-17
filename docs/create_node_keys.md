# Let Node create Keys

<sup>See also: ​[indy-node-container/run][def1]</sup>

1. Fetch the helper scripts from github

        cd ~  # I'm using /home/ubuntu
        git clone https://github.com/hyperledger/indy-node-container.git

2. Create the root of everything seed

        cd indy-node-container/run
        ./generate_random_seeds.sh


    *(The text output is wrong; there is no .cli.env written)*

    **Important: Please make sure to securely store the seed (found in .node.env)**

3. Create a directory for the network and put the name in the.env file

        mkdir lib_indy/swiss_sandbox
        vi .env
        # replace ssi4de with swiss_sandbox
        # replace GS1Germany with your node's name 
        # (e.g. your organisation's name, a short with all capital letters) 


4. Start the node with an INCOMPLETE config (genesis files are missing) to let it create the keys needed to generate the genesis files. So don’t start it in the background (without daemon mode; -d) and stop it (ctrl-C) after the generated keys have been printed on the console. If you wait to long, you will see some error messages, which can be ignored

        sudo docker compose up


5. Now you see in the output the generate keys (based on the seed). It should look like this

        [+] Running 3/3
        ⠿ Network run_default             Created                      
        ⠿ Container indy_node             Created     
        ⠿ Container indy_node_controller  Created                   
        Attaching to indy_node, indy_node_controller
        indy_node             | 2022-05-11T13:48:28+00:00
        indy_node             | INDY_NETWORK_NAME=swiss_sandbox
        indy_node             | INDY_NODE_NAME=HIN
        indy_node             | INDY_NODE_IP=0.0.0.0
        indy_node             | INDY_NODE_PORT=9701
        indy_node             | INDY_CLIENT_IP=0.0.0.0
        indy_node             | INDY_CLIENT_PORT=9702
        indy_node             | INDY_NODE_SEED=[32 characters]
        indy_node             | [...]	 No keys found. Running Indy Node Init...
        indy_node_controller  | 2022-05-11 13:48:29,801|INFO|container_node_control_tool.py|Node control tool is starting up on 0.0.0.0 port 30003
        indy_node_controller  | 2022-05-11 13:48:29,802|DEBUG|container_node_control_tool.py|Waiting for the next event
        indy_node             | Node-stack name is HIN
        indy_node             | Client-stack name is HINC
        indy_node             | Generating keys for provided seed <SEED>
        indy_node             | Init local keys for client-stack
        indy_node             | Public key is <Public Key>
        indy_node             | Verification key is <Verification Key>
        indy_node             | Init local keys for node-stack
        indy_node             | Public key is <Public Key>
        indy_node             | Verification key is <Verification Key>
        indy_node             | BLS Public key is <BLS Public key>
        indy_node             | Proof of possession for BLS key is <Proof of possession>
        indy_node             | [OK]	 Init complete
        indy_node             | [...]	 Setting directory owner to indy


7. Copy the following values and send it to the ledger coordinator:

    - Public Key
    - Verification key
    - BLS Public key
    - Proof of possession for BLS key


[def1]: https://github.com/hyperledger/indy-node-container/tree/main/run