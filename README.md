Bitcoind for Docker
===================

Docker image that runs the Bitcoin bitcoind node in a container for easy deployment.


Requirements
------------

* Physical machine, cloud instance, or VPS that supports Docker running Ubuntu 16.04 or later (*not OpenVZ containers!*)
* At least 200 GB to store the block chain files (and always growing!)
* At least 2 GB RAM + 2 GB swap file


Really Fast Quick Start
-----------------------

One liner for Ubuntu 16.04 LTS machines with JSON-RPC enabled on localhost and adds upstart init script:

    curl https://gitlab.com/eugenekyselev/bitcoind-docker/-/blob/master/bootstrap-host.sh | sh -s trusty


Quick Start
-----------

1. Create a `bitcoind-data` volume to persist the bitcoind blockchain data, should exit immediately.  The `bitcoind-data` container will store the blockchain when the node container is recreated (software upgrade, reboot, etc):

        docker volume create --name=bitcoind-data
        docker run -v bitcoind-data:/bitcoin --name=bitcoind-node -d \
            -p 8333:8333 \
            -p 127.0.0.1:8332:8332 \
            jeksman/bitcoind

2. Verify that the container is running and bitcoind node is downloading the blockchain

        $ docker ps
        CONTAINER ID        IMAGE                         COMMAND             CREATED             STATUS              PORTS                                              NAMES
        d0e1076b2dca        jeksman/bitcoind:latest     "btc_oneshot"       2 seconds ago       Up 1 seconds        127.0.0.1:8332->8332/tcp, 0.0.0.0:8333->8333/tcp   bitcoind-node

3. You can then access the daemon's output thanks to the [docker logs command]( https://docs.docker.com/reference/commandline/cli/#logs)

        docker logs -f bitcoind-node

4. Install optional init scripts for upstart and systemd are in the `init` directory.


Documentation
-------------

* Additional documentation in the [docs folder](docs).
