# Docker File

Docker file created by myself based on nicolaka/netshoot to create many containers Alphine on server hosts for diferrent VRF/tenants in VXLAN BGP EVPN.

List of tools available in this image

```
-    curl 
-    bind-tools 
-    iproute2 
-    iputils 
-    iperf3 
-    tcpdump 
-    mtr 
-    nmap 
-    openssh-client
-    libcap
```

## How to use

### Ubuntu server physical interface configuration

```yml
# /etc/netplan/50-cloud-init.yaml

network:
    ethernets:
        ens2:
            dhcp4: false
            match:
                macaddress: 52:54:00:15:02:ea
            set-name: ens2
        ens3:
            dhcp4: false
            match:
                macaddress: 52:54:00:0a:5c:7c
            set-name: ens3
        ens4:
            dhcp4: true
    version: 2

    bonds:
      bond0:
        interfaces:
          - ens2
          - ens3
        addresses:
          - 172.16.0.11/24


# apply config.

$ sudo netplan apply
```

```sh
# Build Image
docker image build -t neteng:1.0 .


# Create Tenant Network and vlan
sudo docker network create -d macvlan   --subnet=10.10.0.0/24   --gateway=10.10.0.254   -o parent=bond0.100   rede_vxlan_bond_10-10

sudo docker network create -d macvlan   --subnet=10.101.0.0/24   --gateway=10.101.0.254   -o parent=bond0.101   rede_vxlan_bond_VLAN-101


# RUnning containers

docker container run -d   --name NODE2-HOST2   --network rede_vxlan_bond_10-10   --ip 10.10.0.2   --cap-add NET_ADMIN   --cap-add NET_RAW   neteng:1.0

docker container run -d   --name NODE1-HOST1   --network rede_vxlan_bond   --ip 10.100.0.1   --cap-add NET_ADMIN   --cap-add NET_RAW   neteng:1.0

```