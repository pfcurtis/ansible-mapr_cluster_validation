# Overview

This repo contains ansible roles to carry out a basic, fully automatic, cluster validation.

Currently it will do the following.

* Triad memory test for memory throughput
* fio disk test for 1MB sequential throughput for each disk
	* N.B. this is destructive and requires `destructive_test_ok: True` in `group_vars/all`
* MapR rpctest for network bandwidth using MapR RPCs
	* 1:1 (all nodes connect to all other nodes, round robin style)
	* 1:many (each node runs the test to all other nodes)

Optionally it can configure the MapR installer for you as well if you set
`install_mapr_installer: True` in `group_vars/all`.

# Running

To run you need an inventory file with all nodes in it. Please refer to the ansible documentation for how to create the inventory file.

Copy `group_vars/all.example` to `group_vars/all` and edit it to your liking.

This should function even without internet access, except for the MapR installer installation.

By default, this playbook will use the json callback plugin, which will not emit any output until everything is done.

YMMV, but for me against a 4 node cluster with 1 HDD it takes about 20-25 minutes to complete. Larger clusters, more
disks will take longer.

An example to run this with the user `centos` (should have sudo ability on the remotes) on the remote machines, and
writing the output to a file for inspection:

```
ansible-playbook -u centos -i inventory cluster-validation.yml > cluster-validation.json
```

