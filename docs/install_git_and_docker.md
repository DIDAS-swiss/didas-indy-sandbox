# Install Git and Docker in Node VM

<sup>The following instructions have been taken from: [Install Docker Engine on Ubununtu][def1]</sup>

## Set up the repository

1. Update the apt package index and install packages to allow apt to use a repository over HTTPS:

        sudo apt-get update

        sudo apt-get install \
            git \
            ca-certificates \
            curl \
            gnupg \
            lsb-release


2. Add Docker’s official GPG key:

        curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo \
        gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

3. Use the following command to set up the stable repository.

        echo \
        "deb [arch=$(dpkg --print-architecture) \
        signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] \
        https://download.docker.com/linux/ubuntu \
        $(lsb_release -cs) stable" | \
        sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

## Install Docker Engine

1. Update the apt package index, and install the latest version of Docker Engine, containerd, and Docker Compose:

        $ sudo apt-get update
        $ sudo apt-get install docker-ce docker-ce-cli containerd.io \
        docker-compose-plugin

2. Allow the current user to run docker without sudo:

        sudo usermod $LOGNAME --append --groups docker

3. Verify that Docker Engine is installed correctly by running the hello-world image.

        docker run --rm hello-world

4. Cleanup

        docker image rm -f hello-world

[def1]: https://docs.docker.com/engine/install/ubuntu/