<div>
    <img align="center" src="under-the-hood.jpg" />
</div>

# Kubernetes under the hood

## Getting Started

### Prerequisites
The premise is that you already have Virtualbox properly installed on your local machine.

### Download base image
To continue with this demo you need to download the base image and register it in Virtualbox.

```
$ cd ~/VirtualBox\ VMs/

$ wget https://www.dropbox.com/s/v6h0sedqt3za9pl/image-base.tar.bz2?dl=0

$ tar xvjf image-base.tar.bz2

$ vboxmanage registervm ~/VirtualBox\ VMs/image-base/image-base.vbox
```

## Configuring

You need to add the routes on your local machine to access the internal network of Virtualbox.

```
$ sudo ip route add 192.168.1.0/24 via 192.168.1.254
$ sudo ip route add 192.168.2.0/24 via 192.168.2.254
$ sudo ip route add 192.168.3.0/24 via 192.168.3.254
$ sudo ip route add 192.168.4.0/24 via 192.168.4.254
$ sudo ip route add 192.168.254.0/24 via 192.168.254.254
```

If you are a dnsmasq running on your local machine execute this to use private DNS of this DEMO to domain 'kube.local'

```
$ echo "server=/kube.local/192.168.254.254" | sudo tee -a /etc/dnsmasq.d/server
```

## Running

Now let's create the images using a tool (create-image.sh) that will help us clone the base image and add the user-data, meta-data and network-config scripts that cloud-init will use to make the installation of the necessary packages and configurations.

```
$ ./create-image.sh \
    -s or --ssh-pub-keyfile SSH_PUB_KEY_FILE \
    -u or --user-data USER_DATA_FILE \
    -m or --meta-data META_DATA_FILE \
    -n or --network-interfaces NETWORK_INTERFACES_FILE \
    -p or --post-config-interfaces POST_CONFIG_INTERFACES_FILE \
    -o or --hostname HOSTNAME \
    -b or --base-image BASE_IMAGE \
    -a or --auto-start AUTO_START
```

### Parameters:
* __`SSH_PUB_KEY_FILE`__: Path to an SSH public key.
* __`USER_DATA_FILE`__: Path to an user data file. Default is '/data/user-data'.
* __`META_DATA_FILE`__: Path to an meta data file. Default is '/data/meta-data'.
* __`NETWORK_INTERFACES_FILE`__: Path to an network interface data file.
* __`POST_CONFIG_INTERFACES_FILE`__: Path to an post config interface data file.
* __`HOSTNAME`__: Hostname of new image.
* __`BASE_IMAGE`__: Name of VirtualBox base image.
* __`AUTO_START`__: Auto start vm. Default is true.

For more information:
```
$ ./create-image.sh -h or --help
```

## Running Demo

```
$ ./create-image.sh \
    -s ~/.ssh/id_rsa.pub \
    -u ~/Projects/images/data/gate/user-data \
    -n ~/Projects/images/data/gate/network-config \
    -p ~/Projects/images/data/gate/post-config-interfaces \
    -o gate-node01 \
    -b image-base

$ ./create-image.sh \
    -s ~/.ssh/id_rsa.pub \
    -u ~/Projects/images/data/hapx/user-data \
    -n ~/Projects/images/data/hapx/network-config \
    -p ~/Projects/images/data/hapx/post-config-interfaces \
    -o hapx-node01 \
    -b image-base

$ ./create-image.sh \
    -s ~/.ssh/id_rsa.pub \
    -u ~/Projects/images/data/hapx/user-data \
    -n ~/Projects/images/data/hapx/network-config \
    -p ~/Projects/images/data/hapx/post-config-interfaces \
    -o hapx-node02 \
    -b image-base

$ ./create-image.sh \
    -s ~/.ssh/id_rsa.pub \
    -u ~/Projects/images/data/kube/user-data \
    -n ~/Projects/images/data/kube/network-config \
    -p ~/Projects/images/data/kube-mast/post-config-interfaces \
    -o kube-mast01 \
    -b image-base

$ ./create-image.sh \
    -s ~/.ssh/id_rsa.pub \
    -u ~/Projects/images/data/kube/user-data \
    -n ~/Projects/images/data/kube/network-config \
    -p ~/Projects/images/data/kube-mast/post-config-interfaces \
    -o kube-mast02 \
    -b image-base

$ ./create-image.sh \
    -s ~/.ssh/id_rsa.pub \
    -u ~/Projects/images/data/kube/user-data \
    -n ~/Projects/images/data/kube/network-config \
    -p ~/Projects/images/data/kube-mast/post-config-interfaces \
    -o kube-mast03 \
    -b image-base

$ ./create-image.sh \
    -s ~/.ssh/id_rsa.pub \
    -u ~/Projects/images/data/kube/user-data \
    -n ~/Projects/images/data/kube/network-config \
    -p ~/Projects/images/data/kube-node/post-config-interfaces \
    -o kube-node01 \
    -b image-base

$ ./create-image.sh \
    -s ~/.ssh/id_rsa.pub \
    -u ~/Projects/images/data/kube/user-data \
    -n ~/Projects/images/data/kube/network-config \
    -p ~/Projects/images/data/kube-node/post-config-interfaces \
    -o kube-node02 \
    -b image-base

$ ./create-image.sh \
    -s ~/.ssh/id_rsa.pub \
    -u ~/Projects/images/data/kube/user-data \
    -n ~/Projects/images/data/kube/network-config \
    -p ~/Projects/images/data/kube-node/post-config-interfaces \
    -o kube-node03 \
    -b image-base

$ ./create-image.sh \
    -s ~/.ssh/id_rsa.pub \
    -u ~/Projects/images/data/glus/user-data \
    -n ~/Projects/images/data/glus/network-config \
    -p ~/Projects/images/data/glus/post-config-interfaces \
    -o kube-glus01 \
    -b image-base

$ ./create-image.sh \
    -s ~/.ssh/id_rsa.pub \
    -u ~/Projects/images/data/glus/user-data \
    -n ~/Projects/images/data/glus/network-config \
    -p ~/Projects/images/data/glus/post-config-interfaces \
    -o kube-glus02 \
    -b image-base

$ ./create-image.sh \
    -s ~/.ssh/id_rsa.pub \
    -u ~/Projects/images/data/glus/user-data \
    -n ~/Projects/images/data/glus/network-config \
    -p ~/Projects/images/data/glus/post-config-interfaces \
    -o kube-glus03 \
    -b image-base
```
## Contributing

Please read [CONTRIBUTING.md](CONTRIBUTING.md) for details on our code of conduct, and the process for submitting pull requests to us.

## Versioning

We use [GitHub](https://github.com/mvallim/kubernetes-under-the-hood) for versioning. For the versions available, see the [tags on this repository](https://github.com/mvallim/kubernetes-under-the-hood/tags). 

## Authors

* **Marcos Vallim** - *Initial work, Development, Test, Documentation* - [mvallim](https://github.com/mvallim)
* **Fabio Franco Uechi** - *Validation demo* - [fabito](https://github.com/fabito)
* **Dirceu Alves Silva** - *Validation demo* - [dirceusilva](https://github.com/dirceuSilva)
* **Leandro Nunes Fantinatto** - *Validation demo* - [lnfnunes](https://github.com/lnfnunes)
* **Marcos de Lima Goncalves** - *Validation demo, Presentation Organizer* - []()
* **Murilo Woigt Miranda** - *Validation demo, Presentation Organizer* - []()

See also the list of [contributors](CONTRIBUTORS.txt) who participated in this project.

## License

This project is licensed under the BSD License - see the [LICENSE](LICENSE) file for details