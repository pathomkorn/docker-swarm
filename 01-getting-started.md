# Getting Started
* Initialize Docker Swarm
```bash
core@coreos1 ~ $ docker swarm init
Swarm initialized: current node (b4i190tfaymcf30bzoc9ypv6x) is now a manager.

To add a worker to this swarm, run the following command:

    docker swarm join \
    --token SWMTKN-1-2auwecfr9ehickbyd4poviwoldjwfql6youb4y493wfksu80pk-at2hcqbw9qwcv4ws7frjott22 \
    192.168.111.51:2377

To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.
```
* Join first Swarm manager.
```bash
core@coreos1 ~ $ docker swarm join-token manager
To add a manager to this swarm, run the following command:

    docker swarm join \
    --token SWMTKN-1-2auwecfr9ehickbyd4poviwoldjwfql6youb4y493wfksu80pk-dkt6vzsk6s0yc3m4rmzwcc3my \
    192.168.111.51:2377
```
* Join additional 2 Swarm manager.
```bash
core@coreos2 ~ $ docker swarm join --token SWMTKN-1-2auwecfr9ehickbyd4poviwoldjwfql6youb4y493wfksu80pk-dkt6vzsk6s0yc3m4rmzwcc3my 192.168.111.51:2377
This node joined a swarm as a manager.
core@coreos3 ~ $ docker swarm join --token SWMTKN-1-2auwecfr9ehickbyd4poviwoldjwfql6youb4y493wfksu80pk-dkt6vzsk6s0yc3m4rmzwcc3my 192.168.111.51:2377
This node joined a swarm as a manager.
```
