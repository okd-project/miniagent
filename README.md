# Miniagent

This repo contains a number of utility scripts to setup a virtualized OKD cluster using agent-based installer (ABI).
The main goal is to provide a quick and easy playground for deploying OKD and experiment the various features of ABI, 
by using the simplest approach possible (meant for educational/testing purpouses only).

# Limitations

The current scripts are limited to deploy just the Single Node OpenShift (SNO) topology only (so 1 node cluster).

# Pre-requisites

* Requires [libvirt](https://libvirt.org/compiling.html)
    * [virt-manager](https://virt-manager.org/) is recommended
* An ssh key stored in `~/.ssh/id_rsa.pub`

# Getting started

1. Launch the setup script specifying the required release version (see the [release](https://github.com/okd-project/okd/releases) page).

``` bash
$ ./sno-setup.sh quay.io/openshift/okd:4.15.0-0.okd-2024-01-27-070424
```

2. Wait for the installation to complete. The console will show a detailed output about each phase of the installation.

``` bash
...
INFO Cluster is installed                         
INFO Install complete!                            
INFO To access the cluster as the system:admin user when using 'oc', run 
INFO     export KUBECONFIG=/tmp/agent/auth/kubeconfig 
...
```

> **_NOTE:_**  The IP of the node is `192.168.133.80`

3. Connect to your new cluster using the credentials stored in the asset folder.

``` bash
$ export KUBECONFIG=/tmp/mini-agent/auth/kubeconfig
$ kubectl get nodes
NAME       STATUS   ROLES                         AGE   VERSION
master-0   Ready    control-plane,master,worker   36m   v1.28.6+0fb4726
```

Optionally, verify that the cluster has been correctly deployed:

```bash
$ kubectl get clusterversion version
NAME      VERSION                          AVAILABLE   PROGRESSING   SINCE   STATUS
version   4.15.0-0.okd-2024-01-27-070424   True        False         51m     Cluster version is 4.15.0-0.okd-2024-01-27-070424
```

4. Once done, to remove the cluster and cleanup the enviroment simply run the cleanup script.

``` bash
$ ./sno-cleanup.sh
```

# Troubleshooting

To access the node via SSH use the following command.

```bash
$ ssh core@192.168.133.80
```

# Thanks

These scripts were inspired thanks to the experience and contributions of the various authors of [dev-scripts](https://github.com/openshift-metal3/dev-scripts/)
