k8s-on-openstack
================

NOTE TOF: this was zioproto's fork of a k8s deployment automation. Didn't work for us on Ned.


An opinionated way to deploy a Kubernetes cluster on top of an OpenStack cloud.

It is based on the following tools:

  * `kubeadm`
  * `ansible`
  * `weave-net`

Getting started
---------------

The following mandatory environment variables need to be set before calling `ansible-playbook`:

  * `OS_*`: standard OpenStack environment variables such as `OS_AUTH_URL`, `OS_USERNAME`, ...
  * `KEY`: name of an existing SSH keypair

The following optional environment variables can also be set:

  * `STATE`: set to `present` by default and must be set to `absent` to destroy the cluster
  * `NAME`: name of the Kubernetes cluster, used to derive instance names, `kubectl` configuration and security group name
  * `IMAGE``: name of an existing Ubuntu 16.04 image
  * `NETWORK`: name of the network to which instances should be connected
  * `SUBNET_UUID`: UUID of the subnet to which instances should be connected (required for LBaaSv2)
  * `FLOATING_IP_POOL`: name of the floating IP pool
  * `FLOATING_IP_NETWORK_UUID`: uuid of the floating IP network (required for LBaaSv2)
  * `NODE_MEMORY`: how many MB of memory should nodes have, defaults to 4GB
  * `NODE_COUNT`: how many nodes should we provision, defaults to 3
  * `MASTER_BOOT_FROM_VOLUME`: boot the master instance on a volume for data persistence, defaults to True
  * `MASTER_TERMINATE_VOLUME`: delete the volume when master instance is destroy, defaults to True
  * `MASTER_VOLUME_SIZE`: size of the master volume
  * `MASTER_MEMORY`: how many MB of memory should master have, defaults to 4 GB

Spin up a new cluster:

```console
$ ansible-playbook site.yaml
```

Destroy the cluster:

```console
$ STATE=absent ansible-playbook site.yaml
```

CI/CD
-----

The following environment variables needs to be defined:

  * `OS_AUTH_URL`
  * `OS_PASSWORD`
  * `OS_USERNAME`

Author
------

  * François Deppierraz <francois.deppierraz@infraly.ch>

References
----------

  * https://kubernetes.io/docs/getting-started-guides/kubeadm/
  * https://www.weave.works/docs/net/latest/kube-addon/
  * https://github.com/kubernetes/dashboard#kubernetes-dashboard
  
Notes TOF
------------

In order to get it running had to update a few things.
To prevent it from redeploying each time you have to provide an inventory file to ansible:


		ansible-playbook -i hosts.ned.inventory site.yaml
		

 
