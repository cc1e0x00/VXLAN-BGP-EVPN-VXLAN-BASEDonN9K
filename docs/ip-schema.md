# IP and numbering schema
Compact reference for all addressing, ASNs, VLANs, and VNIs used in the fabric. Use this as the source of truth when extending the design.

## ASN allocation


| Role   | ASN   | Notes                          |
|--------|-------|--------------------------------|
| Spine  | 65000 | All fabric shares the same ASN |
| leaf1  | 65000 |                                |
| leaf2  | 65000 |                                |
| leaf3  | 65000 |                                |
| leaf4  | 65000 |                                |

All switches share the same ASN while spine layer hold route reflector role for relex full mesh iBGP requirement.
## Loopback Assigment

### Loopback0 — router ID

Used origin of underlay routing announcements and physical uplink ip unnumered

| Device | Loopback0 IP |
|--------|--------------|
| spine1-rio.dc | 10.0.0.11/32 |
| spine2-rio.dc | 10.0.0.12/32 |
| leaf1-rio.dc  | 10.0.0.13/32 |
| leaf2-rio.dc  | 10.0.0.14/32 |
| leaf3_vpc-rio.dc  | 10.0.0.15/32  |
| leaf4_vpc-rio.dc  | 10.0.0.16/32 |
| borderleaf1-rio.dc  | 10.0.0.17/32 |
| borderleaf2-rio.dc  | 10.0.0.18/32 |



### Loopback1 — VTEP source (leaves only)

Used as the VXLAN tunnel source IP. Separating VTEP from router-id allows distributed-VTEP designs without disrupting BGP.ai-training

| Device | Loopback1 IP  || Anycast gateway IP   | 10.100.0.1/24        |

|--------|---------------|
| spine1-rio.dc | 10.0.0.111/32 |
| spine2-rio.dc | 10.0.0.112/32 |
| leaf1-rio.dc  | 10.0.0.113/32 |
| leaf2-rio.dc  | 10.0.0.114/32 |
| leaf3_vpc-rio.dc  | 10.0.0.115/32 10.0.0.120/32 |
| leaf4_vpc-rio.dc  | 10.0.0.116/32 10.0.0.120/32|
| borderleaf1-rio.dc  | 10.0.0.117/32 |
| borderleaf2-rio.dc  | 10.0.0.118/32 |


Range reserved for future VTEP loopbacks: 10.0.0.125–149.

## Overlay constants

| Parameter            | Value                |
|----------------------|----------------------|
| Edge VLAN            | 900                  |
| Compute VLAN name    | EDGE                 |
| L2VNI                | 900                  |
| Tenant VRF           | EDGE_VRF_L3_VNI      |
| L3VNI                | 30900               |
| Anycast gateway MAC  | 00:1c:73:00:00:01    |
| Compute subnet       | 100.100.0.0/24       |
| MTU (fabric)         | 9214                 |
| MTU (servers)        | 1500                 |
| Docker Network       | -                    |

| Parameter            | Value                |
|----------------------|----------------------|
| Compute VLAN         | 100                  |
| Compute VLAN name    | K8S                  |
| L2VNI                | 1110x                |
| Tenant VRF           | K8S_VRF_L3_VNI       |
| L3VNI                | 30100                |
| Anycast gateway IP   | 10.10x.0.1/24        |
| Anycast gateway MAC  | 00:1c:73:00:00:01    |
| Compute subnet       | 10.10x.0.0/24        |
| MTU (fabric)         | 9214                 |
| MTU (servers)        | 1500                 |
| Desc      | Docker Net.| rede_vxlan_bond_vlan10x|

| Parameter            | Value                |
|----------------------|----------------------|
| Compute VLAN         | 200                  |
| Compute VLAN name    | COMPUTE              |
| L2VNI                | 2220x                |
| Tenant VRF           | COMPUTE_VRF_L3_VNI   |
| L3VNI                | 30200                |
| Anycast gateway IP   | 10.200.0.1/24        |
| Anycast gateway MAC  | 00:1c:73:00:00:01    |
| Compute subnet       | 10.20x.0.0/24        |
| MTU (fabric)         | 9214                 |
| MTU (servers)        | 1500                 |
| Desc.       | UCS Server physical bond0|

## GPU server addressing

| Server   | Attached leaf | Mgmt (eth0)    | Compute (eth1) |
|----------|---------------|----------------|----------------|
| UCS1     | leaf1         | 192.0.2.31 | 10.200.0.11/24 |
| UCS2     | leaf2         | 192.0.2.32 | 10.200.0.12/24 |
| UCS3     | leaf3         | 192.0.2.33 | 10.200.0.13/24 |


All servers default-route to 10.100.0.1 (anycast gateway).

## Management network

Used for SSH, gNMI streaming, Ansible automation, and out-of-band access.

| Function           | Network            |
|--------------------|--------------------|
| Management subnet  | 192.0.2.0/24   |
| Management gateway | 192.0.2.254      |
| automation-host    | 192.0.2.232 (DHCP)  |

### Switch management IPs

| Device | Management IP   |
|--------|-----------------|
| spine1-rio.dc | 192.0.2.11  |
| spine2-rio.dc | 192.0.2.12  |
| leaf1-rio.dc  | 192.0.2.13  |
| leaf2-rio.dc  | 192.0.2.14  |
| leaf3_vpc-rio.dc  | 192.0.2.15  |
| leaf4_vpc-rio.dc  | 192.0.2.16  |
| borderleaf1-rio.dc  | 192.0.2.17  |
| borderleaf2-rio.dc  | 192.0.2.18  |

## Route-target convention

Route-targets follow the pattern `<VNI>:<VNI>`:

| Resource                  | Route-target  |
|---------------------------|---------------|
| L2VNI 900 (Edge VLAN)     | 65000:900   |
| L3VNI 309000 (tenant VRF) | 65000:30900 |
| L2VNI 1110x (K8s VLAN)    | 65000:1110x   |
| L3VNI 50000 (tenant VRF)  | 65000:30100   |
| L2VNI 2220x (Compute VLAN)| 65000:2220x   |
| L3VNI 50000 (tenant VRF)  | 65000:30200   |
