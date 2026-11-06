# About the project 

This project is part of notes I've been done to document my progress on Cisco's Data center pespective.Feel free to check not only Data Center topics but also  my journey about CCIE EI completed in 2023.

[Medium Profile: CC1E0x108D4](https://rafaesil.medium.com/)


Addition to protocol and lab notes, like the first part of [VXLAN Flood & Learning article](https://rafaesil.medium.com/dc-journey-day-6-vxlan-flood-and-learn-on-nx-os-be3b25c9c381?sharedUserId=rafaesil). I will share my VXLAN BGP EVPN deployment plus IaC using ansible that's automation tool I'm gooing deeper.

# VXLAN-BGP-EVPN-BASEDonN9K

## Essentials files and a structure way to organize the project


```
├── Docker
│   ├── Dockerfile
│   └── README.md
├── README.md
├── imagens
│   └── BGP-EVPN-VXLAN-BASED_N9k.jpg
├── knowlegment
│   └── VXLAN BGP EVPN.md
└── network-automation
    └── ansible
        ├── ansible.cfg
        ├── inventory
        │   ├── group_vars
        │   ├── host_vars
        │   └── prod.yml
        ├── playbooks
        └── roles
```

## Network Topology
![Digram](imagens/BGP-EVPN-VXLAN-BASED_N9k.jpg)