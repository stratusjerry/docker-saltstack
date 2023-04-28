# docker-saltstack
Docker Compose setup to spin up a salt master and minions.

You will need a system with Docker and Docker Compose installed to use this project.

Just run:

```bash
# Start in interactive mode, outputting to screen
docker-compose up
# Start in background mode
docker-compose up -d
# Stop the containers
docker-compose stop

```

from a checkout of this directory, and the master and minion will start up with debug logging to the console.

Log into the command line of the salt-master server.
`docker-compose exec salt-master bash`

From that command line you can run something like:

`salt '*' test.ping`

and in the window where you started docker compose, you will see the log output of both the master sending the command and the minion receiving the command and replying.

Note: you will see log messages like : "Could not determine init system from command line" - those are just because salt is running in the foreground and not from an auto-startup.

The salt-master is set up to accept all minions that try to connect.  Since the network that the salt-master sees is only the docker-compose network, this means that only minions within this docker-compose service network will be able to connect (and not random other minions external to docker).

#### Host Names
The **hostnames** match the names of the containers - so the master is `salt-master` and the minion is `salt-minion`.

If you are running more than one minion with `--scale=2`, you will need to use `docker-saltstack_salt-minion_1` and `docker-saltstack_salt-minion_2` for the minions if you want to target them individually.
