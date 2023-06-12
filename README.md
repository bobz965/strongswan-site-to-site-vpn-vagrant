This is a [Vagrant](https://www.vagrantup.com/) Environment for a IPsec VPN device based on [strongSwan](https://strongswan.org).

These are the machines and how they are connected with each other:

<img src="diagram.png">


# Usage

Build and install the [Ubuntu Base Box](https://github.com/rgl/ubuntu-vagrant).

Run `vagrant up` to launch everything.

Login into the moon machine (a VPN device), and watch the network traffic:

    tcpdump -n -i any tcp port 3000

From your host computer, access the following URLs to see them working:

    http://10.1.0.4:3000
    http://10.2.0.4:3000

Then, ssh into the moon-ubuntu machine, and try it there:

    vagrant ssh moon-ubuntu
    wget -qO- 10.2.0.4:3000


# Reference

* https://wiki.strongswan.org/projects/strongswan/wiki/IntroductionTostrongSwan



# vagrant up 问题
## 1. 缺少 box
https://app.vagrantup.com/peru/boxes/ubuntu-18.04-server-amd64

``` bash 
### 不知道项目原来的镜像是哪里的
vagrant  box  add  peru/ubuntu-18.04-server-amd64 

mkdir "ubuntu-18.04-server-amd64" && cd "ubuntu-18.04-server-amd64" || exit
vagrant init "peru/ubuntu-18.04-server-amd64"
vagrant up
virsh list --all
vagrant ssh
vg destroy

```

## 2. nfs udp

``` bash
apt install nfs-kernel-server -y
# nfsd enable udp
cat /etc/nfs.conf| grep -n udp
30:# udp-port=0
57:udp=y

systemctl restart rpcbind nfs-kernel-server


mount -t nfs -o resvport,rw 192.168.121.1:/root/strongswan-site-to-site-vpn-vagrant /vagrant
  
umount /vagrant




```





