# docker-saltstack
Docker Compose setup to create a salt master and several minions.

# Pre-Reqs
You will need Docker and Docker Compose installed to use this project.  See Linux instructions for installing [Docker](https://docs.docker.com/engine/install/#server) and [Docker Compose](https://docs.docker.com/compose/install/linux/). If using Mac or Windows, use [Docker Desktop](https://www.docker.com/products/docker-desktop/).

Additionally, you will need [git](https://github.com/git-guides/install-git) installed.

# Setup
See setup instructions in comments below
```bash
# Create a directory on your machine to clone this repo to
mkdir -p /local/demo/
cd /local/demo/
# Use git to clone this repo to your local directory
git clone https://github.com/stratusjerry/docker-saltstack.git
cd /local/demo/docker-saltstack
# Start the salt master and minion containers (this may take several minutes to provision)
docker-compose up -d
```

Once the containers have started, you can log into the command line of the salt-master server by running
```bash
docker-compose exec salt-master bash
```

From within the salt-master, you can run salt commands like
```bash
# Run a salt 'ping' test on all minions
salt '*' test.ping
# Feel free to run more salt master commands on minions 
#   and when finished, exit the salt master container with the
#   exit command
exit
```

To stop the running containers:
```bash
# Make sure you are in the directory the 
cd /local/demo/docker-saltstack
# Run the ommand below to stop the running containers
docker-compose stop
# If you wish to permanately delete the containers and there base image, run the command
docker-compose down --rmi -v
```



Note: you will see log messages like : "Could not determine init system from command line" - those are just because salt is running in the foreground and not from an auto-startup.

The salt-master is set up to accept all minions that try to connect.  Since the network that the salt-master sees is only the docker-compose network, this means that only minions within this docker-compose service network will be able to connect (and not random other minions external to docker).

# Debugging
```bash
# Render Output, good for testing YAML anchors
docker-compose config > ./testing/render.yml

```
