# Introduction

Windows and linux networking tips and tricks

# Contents
[[_TOC_]]

# Quick Troubleshoot
* Can systems ping?
* Is there a firewall?
* Does there need to be NAT somewhere?
* Can each side route to the other? `tracert`/`traceroute` is good for this.

# Sniffing

## Ettercap

Ettercap is used to ARP poison a network to perform Man-in-the-Middle (MITM) attacks. A simple MITM attack targeting the entire subnet can be executed as follows:

* For ettercap compiled with IPv6 support:

    ```
    ettercap -M arp -q -T -i eth0 /// ///    
    ```

* For an older version of ettercap with only IPv4 support:

    ```
    ettercap -M arp -q -T -i eth0 // //
    ```

### Command Breakdown
* `-M`: The attack to execute. In this case we're performing an `arp` poisoning attack (_TODO talk about other ettercap modes_)
* `-q` (optional): Stands for quiet. Means that ettercap won't output all the packets by defaults. Press the space bar to toggle between "quiet" and "loud" once the tool is running.
* `-T`: Text mode
* `-i`: The interface to use. Adjust this option if needed.
* `//` and `//` or `///` and `///`: _TODO: Explain the filters in ettercap_

## Wireshark

### Display Filters

### Capture Filters

## BPF Filters (`tcpdump`/scapy)

| Command | Description |
| ------- | ----------- |
| `man pcap-filter` | View man page for filter format |

# Windows

_IDEAS: how to set static IPs, how to enable/disable firewall rules, etc..._

# Linux

## Management

## `tcpdump`

## Firewalls

> Not all tested

| Command | iptables | firewalld (RHEL/Centos) | ufw (Ubuntu) |
| ------- | -------- | ----------------------- | ------------ |
| Set changes | Automatic | `firewall-cmd --reload` | Automatic |
| Add port | `iptables -I INPUT 1 -p <tcp/udp> --dport <PORT> -j ACCEPT` | `firewall-cmd --zone=public  --permanent --add-port=<PORT>/<tcp/udp>` | `ufw allow proto <tcp/udp> from any to any port <PORT>` |
| Allow everything from host | `iptables -I INPUT 1 -s <HOST> -j ACCEPT` | `firewall-cmd --permanent --zone=public --add-source=<HOST>/32` | `ufw allow from <HOST> to any` |
| Port forwarding | `iptables -t nat -A PREROUTING -p tcp -d <DEST_IP> --dport <ORIG_PORT -j REDIRECT --to-ports <TO_PORT>` | `firewall-cmd --permanent --zone=public --add-forward-port=port=<ORIG_PORT>:proto=tcp:toport=<TO_PORT>` | Use iptables |
| Enable NAT/Masquerade | `iptables -t nat -A POSTROUTING -o <OUT_INTERFACE> -j MASQUERADE` | `firewall-cmd --permanent --zone=public --add-masquerade` | Use iptables |

### Management Commands


| Description | New `iproute2` | Older `net-tools` |
| ----- | -------- | -------- |
| Create a tap interface (virtual interface) | `ip tuntap add name tap0 mode tap` |  |
| Show IP link info | `ip link show` | `ifconfig` |
| Show routing table | `route -n` | `ip route` |
| Show ARP table | `ip neigh` | `arp -a` |
| Bring interface up/down | `ip link set <INTERFACE> <up/down>` | `ifconfig <INTERFACE> <up/down>` |
| Set interface IP manually | `ip address add <ADDRESS>/<CIDR> dev <INTERFACE>` | `ifconfig <INTERFACE> <ADDRESS> netmask <NETMASK>` |
| Remove IP address | `ip addr del <ADDRESS>/<CIDR> dev eth0` | `ifconfig <INTERFACE> 0.0.0.0` |
| Show all interfaces with addresses | `ip addr` | `ifconfig` |
| Show interface info | `ip link show <INTERFACE>` | `ifconfig <INTERFACE>` |
| Add default gateway | `ip route add default via <GW>` | `route add default gw <GW>` |
| Add route (interface) | `ip route add <NETWORK_WITH_CIDR> dev <OUT_INTERFACE>` | `route add -net <NETWORK_WITH_CIDR> dev <OUT_INTERFACE>` |
| Add route (IP) | `ip route add <NETWORK_WITH_CIDR> via <IP>` | `route add -net <NETWORK_WITH_CIDR> gw <IP>` |
| Delete route (interface) | `ip route del <NETWORK_WITH_CIDR> dev <OUT_INTERFACE>` | `route del -net <NETWORK_WITH_CIDR> dev <OUT_INTERFACE>` |
| Delete route (IP) | `ip route del <NETWORK_WITH_CIDR> via <IP>` | `route del -net <NETWORK_WITH_CIDR> gw <IP>` |