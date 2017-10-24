# Internet layer


The network layer is responsible for being able to communicate with other hosts on the network. In order to be able to communicate, each host should have *three* settings configured correctly:

1. The network interface should have an **IP address** assigned
2. A **default gateway** should be set
3. A **DNS server** should be set

Before "reaching out" to other hosts, first check local settings.

## IP address

The IP address may have been set automatically (DHCP), or manually. Check this in `/etc/sysconfig/network-scripts/ifcfg-IFACE`, with IFACE the name of the network interface.

Use the command `ip address` (or shortcut `ip a`) to list the IP addresses for each interface.

You should know the expected value, if not the exact IP, at least the network range or network IP and network mask.

- Many home routers have an IP range of 192.168.0.0/24, 192.168.1.0/24
- On a VirtualBox NAT interface, the IP address of the VM should be 10.0.2.15/8
- On a VirtualBox host-only interface, the IP address of the VM depends on how you configured this network. The *default* host only network has IP range 192.168.56.0/24, with a DHCP server handing out addresses starting at 192.168.56.101.

Possible problems and causes (automatic IP assignment with DHCP):

- No IP
    - The DHCP server is unreachable
    - The DHCP server won't give an IP to this host
- IP looking like 169.254.x.x
    - No DHCP server could be reached, and a "link-local" address was assigned
- IP not in the expected range
    - You may have accidentally set a fixed IP previously, and forgot to set it to DHCP again...

Possible problems and causes (manual IP setting):

- IP not in the expected range
    - Mistake in IP address assignment, check the configuration file
- Correct IP, but "network unreachable"
    - Check the network mask, this should be identical for all hosts on the LAN

## Default gateway

Usually, a host is connected to a LAN through a switch. Network traffic to the outside world goes through a router, connected to the same LAN. Every host on the LAN should know this router, the "default gateway".

Use the command `ip route` (or shortcut `ip r`). There should be a line starting with `default via x.y.z.w`.

Possible problems and causes (automatic IP assignment with DHCP):

- No default gateway set
    - there is most probably also a problem with the IP address of this host, fix this first
    - If you manage the DHCP server, maybe it's configured badly?
- Unexpected default gateway address
    - You may have accidentally set the default gateway manually previously and forgot to set it to DHCP again
    - If you manage the DHCP server, maybe it's configured badly?

## DNS server

In order to be able to resolve host names to IP addresses, every host should be able to contact a DNS server. View the file `/etc/resolv.conf`. It usually has a header that mentions it was generated automatically, and should have one or more lines starting with `nameserver`.

```
# Generated by NetworkManager
```

Possible problems and causes are equivalent to those with the default gateway setting.

## Check LAN connection

If the previous settings are correct, you can check whether other hosts on the LAN can be reached.

- Ping the default gateway: `ping a.b.c.d`
- Ping another known host on the LAN
- Check DNS name resolution: `dig www.google.com @e.f.g.h +short`

**Be aware** that `ping` (and other network troubleshooting tools like the `traceroute` family) may not always work. Some system administrators will block ICMP traffic on routers, rendering the results useless. A command like `ping www.google.com` (for some the first command they try in case of network connection problems) is not very suitable, in that it depends on too many things to work at once:

- the host's network settings should be correct
- routing should work
- DNS should be available
- no router between this host and the target should block ICMP
- ...
