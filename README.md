# autoscaler-docker-swarm

Service to scale up and down containers in swarm depending on the load.

It is one of the features, lack of which swarm makes k8s a no brainer for many use cases. The plan is to make a simple stack addon that will deploy on swarm manager and control replicas of other services based on CPU usage.

## Features

- [x] Autoscaler stack
- [ ] Add support to check disk, ram and other parameters to make sure server has resources to scale up the service

It is inspired from [Stack-Answer](https://stackoverflow.com/questions/41668621/how-to-configure-autoscaling-on-docker-swarm) and derived from this [repo](https://github.com/jcwimer/docker-swarm-autoscaler)
## Run Locally

Create Docker Swarm with HyperV

```bash
  vagrant up
```

Run the `docker swarm join` on the worker for example

`docker swarm join --token SWMTKN-1-2f6t3m2ri12kds1wtw9hw98qirwm26ytkox0s9caobxb38js4c-brhh03xz83pi0wh18a7fr5feg $IP_MANAGER:2377`

Connect to manager and verify if nodes exists

`docker node ls`

Clone the project

```bash
  git clone https://github.com/ArturiaPendragon/autoscaler-docker-swarm
```

Go to the project directory

```bash
  cd autoscaler-docker-swarm
```

Deploy the stack

```bash
  docker stack deploy -c swarm-autoscaler-stack.yml scaling
```
