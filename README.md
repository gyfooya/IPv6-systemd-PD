# IPv6-systemd-PD
IPv6 Prefix Delegation with systemD

                       +------------------------------------+
                       |      ISP / Box                     |
                       |  - IPv6: 2a01:xxx:xxx:88/56        |
                       |  - IPv6  2a01:xxx:xxx:8801/64 +RA  |
                       |  - IPv4 DHCP                       |
                       +-----------+------------------------+
                                   |
                                   | WAN link IPv6
                                   | LAN link IPv4
                                   |
                          +--------v---------+
                          |   PHY NIC Card   |
                          |     enp45s0      |
                          +--------+---------+
                                   |
                         +---------+---------+
                         | Linux Host / OS   |
                         |  (bridge setup)   |
                         +----+---------+----+
                              |         |
                 -------------          -----------
                 |                                |
        +--------v--------------+        +--------v-------------+ +-------------------------+
        |    bridge br0         |        |    bridge br7        | |  radvd                  |
        |  (L2 switch)          |        |  (L2 switch)         | |  router advertisement   |
        |  IPv6 8801/64 SLAAC+RA|        |  IPv6 8807/64        | |  (RA) for prefix        |
        |  IPv4 from DHCP       |        |                      | |  2a01:xxx:xxx:8807::/64 |
        +-----------------------+        +----------------------+ +-------------------------+
                                                     |
                                                     |
                                         +-----------v----------+
                                         |     virt-manager     |
                                         |       VM + LXC       |
                                         +----------------------+

# SystemD Network
- /etc/systemd/network/20-enp45s0.network
- /etc/systemd/network/30-br0.netdev
- /etc/systemd/network/37-br7.netdev
- /etc/systemd/network/40-br0.network
- /etc/systemd/network/47-br7.network

# Radvd Router advertisement
- /etc/radvd.conf

# Sysctl (modifiy kernel parameters at runtime)
- /etc/sysctl/99-ipv6.conf
