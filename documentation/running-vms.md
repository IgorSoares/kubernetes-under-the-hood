## Running

Now let's create the images using a tool ([`create-image.sh`](/create-image.sh)) that will help us clone the base image and add the user-data, meta-data and network-config scripts that cloud-init will use to make the installation of the necessary packages and configurations.

```
$ ./create-image.sh \
    -k or --ssh-pub-keyfile SSH_PUB_KEY_FILE \
    -u or --user-data USER_DATA_FILE \
    -m or --meta-data META_DATA_FILE \
    -n or --network-interfaces NETWORK_INTERFACES_FILE \
    -i or --post-config-interfaces POST_CONFIG_INTERFACES_FILE \
    -s or --post-config-storages POST_CONFIG_STORAGES_FILE \
    -r or --post-config-resources POST_CONFIG_RESOURCES_FILE \
    -o or --hostname HOSTNAME \
    -b or --base-image BASE_IMAGE \
    -l or --linux-distribution LINUX_DISTRIBUTION \
    -a or --auto-start AUTO_START
```

### Parameters:
* __`SSH_PUB_KEY_FILE`__: Path to an SSH public key.
* __`USER_DATA_FILE`__: Path to an user data file. Default is '/data/user-data'.
* __`META_DATA_FILE`__: Path to an meta data file. Default is '/data/meta-data'.
* __`NETWORK_INTERFACES_FILE`__: Path to an network interface data file.
* __`POST_CONFIG_INTERFACES_FILE`__: Path to an post config interface data file.
* __`POST_CONFIG_STORAGES_FILE`__: Path to an post config storage data file.
* __`POST_CONFIG_RESOURCES_FILE`__: Path to an post config resources data file.
* __`HOSTNAME`__: Hostname of new image.
* __`BASE_IMAGE`__: Name of VirtualBox base image.
* __`LINUX_DISTRIBUTION`__: Name of Linux distribution. Default is 'debian'.
* __`AUTO_START`__: Auto start vm. Default is true.

For more information:
```
$ ./create-image.sh -h or --help
```

## Running Demo

All VM initializations and configurations use [**`cloud-init`**](/documentation/cloud-init.md), all YAML scripts are in the [data](/data) directory for the linux distribution used by VMs.

### Create gateway
```
$ ./create-image.sh \
    -k ~/.ssh/id_rsa.pub \
    -u gate/user-data \
    -n gate/network-config \
    -i gate/post-config-interfaces \
    -r gate/post-config-resources \
    -o gate-node01 \
    -l debian \
    -b debian-base-image
```

> Wait the gate-node01 finish the configuration and start VM, to the next steps.

### Create HAProxy Cluster

```
$ for instance in hapx-node01 hapx-node02; do
    ./create-image.sh \
        -k ~/.ssh/id_rsa.pub \
        -u hapx/user-data \
        -i hapx/post-config-interfaces \
        -r hapx/post-config-resources \
        -o ${instance} \
        -l debian \
        -b debian-base-image
done
```

### Create Kubernete Masters
```
$ for instance in kube-mast01 kube-mast02 kube-mast03; do
    ./create-image.sh \
        -k ~/.ssh/id_rsa.pub \
        -u kube/user-data \
        -i kube-mast/post-config-interfaces \
        -r kube-mast/post-config-resources \
        -o ${instance} \
        -l debian \
        -b debian-base-image
done
```

### Create Kube Workers
```
$ for instance in kube-node01 kube-node02 kube-node03; do
    ./create-image.sh \
        -k ~/.ssh/id_rsa.pub \
        -u kube/user-data \
        -i kube-node/post-config-interfaces \
        -r kube-node/post-config-resources \
        -o ${instance} \
        -l debian \
        -b debian-base-image
done
```

### Create Gluster Nodes
```
$ for instance in glus-node01 glus-node02 glus-node03; do
    ./create-image.sh \
        -k ~/.ssh/id_rsa.pub \
        -u glus/user-data \
        -i glus/post-config-interfaces \
        -s glus/post-config-storages \
        -r glus/post-config-resources \
        -o ${instance} \
        -l debian \
        -b debian-base-image
done
```