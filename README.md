Ansible Role: dnsmasq
=========

[![CI](https://github.com/ricsanfre/ansible-role-dnsmasq/actions/workflows/ci.yml/badge.svg)](https://github.com/ricsanfre/ansible-role-dnsmasq/actions/workflows/ci.yml)

Install and configure lightweight DHCP and DNS server, dnsmasq, on Linux.

Requirements
------------

None.

Role Variables
--------------

Available variables are listed below along with default values (see `defaults\main.yaml`)

Host interface and IP where Dnsmasq is listening:

```yml
dnsmasq_interface: ''
dnsmasq_listen_address: ''
```
> Default values are null. Role get interface and ip information from gathered facts.
> In case that the server has more than one interface, specify values for these variables

Local domain name:
```yml
dnsmasq_domain_name: example.ricsanfre.com
```
DNS relay servers:
```yml
home_dns_servers:
  - 80.58.61.250
  - 80.58.61.254
```
DHCP lease IP range:
```yml
dnsmasq_dhcp_range: '10.0.0.32,10.0.0.128'
```

Import DHCP/DNS configuration from inventory for group
```yml
host_groups: all
```
By default it is added to the DHCP and DNS configuration records from the hosts within inventory belonging to group `host_groups`

Variables: `ip`, `mac` and `hostname` need to be added to the hosts in the inventory:
```yml
hosts:
  all:
    children:
      cluster:
        hosts:
          server1:
            hostname: server1
            ip: 10.0.0.11
            mac: dc:a6:32:9c:29:b9
          server2:
            hostname: server2
            ip: 10.0.0.12
            mac: e4:5f:01:2d:fd:19
          server3:
            hostname: server3
            ip: 10.0.0.13
            mac: e4:5f:01:2f:49:05
```
Additional DHCP and DNS records can be added with the following variables:

```yml
additional_dhcp_hosts: {}
```
```yml
additional_dhcp_hosts:
  ethernet_switch:
    desc: "Ethernet Switch"
    mac: 94:a6:7e:7c:c7:69
    ip: 10.0.0.2
```
```yml
additional_dns_hosts: {}
```
```yml
additional_dns_hosts:
  ntp_server:
    desc: "NTP Server"
    hostname: ntp
    ip: 10.0.0.1
  dns_server:
    desc: "DNS Server"
    hostname: dns
    ip: 10.0.0.1
```

Dependencies
------------

None

Example Playbook
----------------


```yml
---
- name: Dnsmasq
  hosts: host
  vars:
    - additional_dhcp_hosts:
        ethernet_switch:
          desc: "Ethernet Switch"
          mac: 94:a6:7e:7c:c7:69
          ip: 10.0.0.2
    - additional_dns_hosts:
        ntp_server:
          desc: "NTP Server"
          hostname: ntp
          ip: 10.0.0.1
        dns_server:
          desc: "DNS Server"
          hostname: dns
          ip: 10.0.0.1
  roles:
    - role: ricsanfre.dnsmasq
```





License
-------

MIT/BSD

Author Information
------------------

Ricardo Sanchez (ricsanfre)
