# Overview

This repo contains ansible roles to carry out a basic, fully automatic, cluster validation.

Currently it will do the following.

* Triad memory test for memory throughput
* fio disk test for 1MB sequential throughput for each disk
* MapR rpctest for network bandwidth using MapR RPCs
	* 1:1 (all nodes connect to all other nodes, round robin style)
	* 1:many (each node runs the test to all other nodes)

# Running

To run you need an inventory file with all nodes in it. Please refer to the ansible documentation for how to create the inventory file.

An example to run this with the user `centos` on the remote machines:

```
ansible-playbook -u centos -i inventory cluster-validation.yml
```


