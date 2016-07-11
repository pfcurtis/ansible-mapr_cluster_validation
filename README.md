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

# Getting it

Go to https://github.com/vicenteg/ansible-mapr_cluster_validation/releases and get a release, preferably the most recent one.

Unpack the tarball.

# Running

To run you need an inventory file with all nodes and a `cluster` group in it. Please refer to the [ansible documentation](http://docs.ansible.com/ansible/intro_inventory.html#hosts-and-groups) for how to create the inventory file.

A simple example:

```
node1 ansible_ssh_host=192.168.101.1
node2 ansible_ssh_host=192.168.101.2
node3 ansible_ssh_host=192.168.101.3

[cluster]
node[1:3]

```

Copy `group_vars/all.example` to `group_vars/all` and edit it to your liking.

This should function even without internet access, except for the MapR installer installation, which is optional and disabled by default.

YMMV, but for me against a 4 node cluster with 1 HDD it takes about 20-25 minutes to complete. Larger clusters, more
disks, slower networks and disks will cause this to take longer.

An example to run this with the user `centos` (should have sudo ability on the remotes) on the remote machines:

```
ansible-playbook -u centos -i inventory cluster-validation.yml
```

If you do not have ssh set up for public key authentication you can run ansible as follows (note the `-k`):

```
ansible-playbook -u centos -k -i inventory cluster-validation.yml
```

Note that this ships with an ansible.cfg that can be modified if, for example, you do not prefer to keep host key
checking enabled. You can also enable JSON output of the playbooks, which will allow you to deal with the output 
programatically.

# Developing

Clone this repo, then `git submodule update --init`.

Create a new branch, e.g., `git checkout -b my_new_feature`.

Commit, push, pull request.

# Packaging

After cloning and initializing submodules:

```
(
	ver=`git tag -l | tail -1`;
	git checkout $ver;
	cd ../;
	tar --exclude all --exclude .git* --exclude *.json --exclude *.retry -cvzf ansible-mapr_cluster_validation-$ver.tar.gz ansible-mapr_cluster_validation
)
```
Then create a new release on github and upload the tarball.
