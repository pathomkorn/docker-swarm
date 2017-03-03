# Getting Started
## Create Docker Swarm
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
* Verify swarm managers
```bash
core@coreos1 ~ $ docker node ls
ID                           HOSTNAME              STATUS  AVAILABILITY  MANAGER STATUS
0in8h9ghhe4oaoqr2k33sim1g    coreos2.ibmcloud.com  Ready   Active        Reachable
604vkaboh7hojh79beblijaj9    coreos3.ibmcloud.com  Ready   Active        Reachable
b4i190tfaymcf30bzoc9ypv6x *  coreos1.ibmcloud.com  Ready   Active        Leader
```
## Create Overlay Network
* Initialize overlay network
```bash
core@coreos1 ~ $ docker network create --driver overlay net1
4y6h9svaxhhzb2qvlzk4pqgh7
```
* Verify overlay network
```bash
core@coreos1 ~ $ docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
55bf8225d7d7        bridge              bridge              local
444c8058cc23        docker_gwbridge     bridge              local
2ff14f4b5c09        host                host                local
0e9iqntyfcvb        ingress             overlay             swarm
4y6h9svaxhhz        net1                overlay             swarm
88b3e3344942        none                null                local
```
# Deploy Container Cluster
* Deploy httpd cluster
```bash
core@coreos1 ~ $ docker service create --name httpd-cluster --replicas 3 -p 80:80 --network net1 httpd
97pwlz7lzpclwu468ucb0zfd9
```
* Verify httpd cluster
```bash
core@coreos1 ~ $ docker service ls
ID            NAME          REPLICAS  IMAGE                               COMMAND
5id2csc7d5oy  httpd-cluster 0/3       httpd
core@coreos1 ~ $ docker service ls
ID            NAME           REPLICAS  IMAGE  COMMAND
97pwlz7lzpcl  httpd-cluster  0/3       httpd
core@coreos1 ~ $ docker service ps httpd-cluster
ID                         NAME             IMAGE  NODE                  DESIRED STATE  CURRENT STATE           ERROR
7rxs4j3p3ykh1khj5dz3xyjnz  httpd-cluster.1  httpd  coreos2.ibmcloud.com  Running        Running 7 seconds ago
5q0j9g08m556oh6ry91r7jrgy  httpd-cluster.2  httpd  coreos3.ibmcloud.com  Running        Running 48 seconds ago
9sl8ylcytg8e8iupeewzszt62  httpd-cluster.3  httpd  coreos1.ibmcloud.com  Running        Running 6 seconds ago
core@coreos1 ~ $ curl http://127.0.0.1
<html><body><h1>It works!</h1></body></html>
core@coreos2 ~ $ curl http://127.0.0.1
<html><body><h1>It works!</h1></body></html>
core@coreos3 ~ $ curl http://127.0.0.1
<html><body><h1>It works!</h1></body></html>
```
