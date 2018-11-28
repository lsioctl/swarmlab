# Swarmlab

A lab to play with Docker Swarm on your workstation

The main idea was to explore Docker networking as it was
done in this amazing article:

https://neuvector.com/network-security/docker-swarm-container-networking/

labs notes are in labs.md

## TODO

* I use curently coreos-vagrant, but it would be nicer to use a simple
Ubuntu with Docker, with Vagrant and maybe Ansible
* Automate the startup commands with Ansible

### Startup 

* edit the number of nodes in coreos-vagrant config.rb
```
$num_instances=2
```
* expose docker port of the nodes via config.rb
```
$expose_docker_tcp=2378
```
it will expose node01 on 2378, node02 on 2379, ...
* start the nodes

```
cd coreos-vagrant
vagrant up
```

* ssh on the nodes and setup the cluster
```
vagrant ssh core-xx
docker swarm init ...
docker swarm join ...
```

* on the nodes ensure that docker is enabled 
```
vagrant ssh core-xx
systemctl enable docker.service
```

* on your workstation, set node01 (manager) as Docker Host
```
export DOCKER_HOST="tcp://127.0.0.1:2378"
```

### Deploy the registry

* tag the master
```
docker node update --label-add registry=true core-01 
```

the Dockerfile will deploy the registry only on the master

* deploy the registry on the swar
```
docker stack deploy -c docker-compose.yml reg
```

### Deploy or update the stacks


cd stackdir (prometheus or app)

docker-compose build (if needed)
docker-compose push (if needed)
docker stack deploy -c docker-compose.yml stack name
