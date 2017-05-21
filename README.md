openwrt-firewall
================

configure firewall aspects of your openwrt system.
compare: [http://wiki.openwrt.org/doc/uci/firewall]

Dependencies
------------

* [openwrt-uci](https://github.com/flandiGT/openwrt-uci)
* python installed (if setup custom rules)

Role Variables
--------------

| variable name     | type             | element structure    | default |
|-------------------|------------------|----------------------|---------|
| zones             | array of objects | see attributes below | []      |
| forwardings       | array of objects | see attributes below | []      |
| redirects         | array of objects | see attributes below | []      |
| rules             | array of objects | see attributes below | []      |
| custom_rules      | array of objects | see attributes below | []      |

Role Variable elements
----------------------

firewall-zone attributes:

| attribute name | property type       | valid values / examples            |
|----------------|---------------------|------------------------------------|
| index          | number              | optional: index of the zone        |
| name           | text                | zone name like 'zone-1' or 'guest' |
| network        | text                | network name this zone belongs to  |
| forward        | option as text      | ACCEPT/DROP/REJECT                 |
| input          | option as text      | ACCEPT/DROP/REJECT                 |
| output         | option as text      | ACCEPT/DROP/REJECT                 |
| masq           | boolean             | True/False                         |
| syn_flood      | boolean             | True/False                         |
| conntrack      | boolean             | True/False                         |

firewall-forwarding attributes:

| attribute name | property type       | valid values / examples            |
|----------------|---------------------|------------------------------------|
| index          | number              | optional: index of the forwarding  |
| src            | network as text     | source network name                |
| dest           | network as text     | destination network name           |

firewall-redirect attributes

| attribute name | property type       | valid values / examples                     |
|----------------|---------------------|---------------------------------------------|
| index          | number              | optional: index of the redirect             |
| name           | text                | redirect role name like 'ssh_nat_redirect ' |
| src            | network as text     | source network (like: 'wan')                |
| proto          | option as text      | selected protocols: tcp / udp / tcpudp      |
| src_dport      | number              | external port (0 - 65535)                   |
| dest_ip        | ip address as text  | private IP address of your target           |
| dest_port      | number              | internal port (0 - 65535)                   |
| target         | option as text      | target (like: 'DNAT')                       |
| dest           | network as text     | destination network (like: 'lan')           |

firewall-rule attributes

| attribute name | property type       | valid values / examples                     |
|----------------|---------------------|---------------------------------------------|
| index          | number              | optional: index of the rule                 |
| name           | text                | rule name like 'my_custom_block_rule        |
| family         | option as text      | ipv4/ipv6 or empty for both                 |
| src            | network as text     | source network (blank for any)              |
| src_ip         | ip address as text  | source ip address (blank for any)           |
| src_port       | number              | source port (blank for any)                 |
| dest           | network as text     | destination network (blank for any)         |
| dest_ip        | ip address as text  | destination ip address (blank for any)      |
| dest_port      | number              | destination port (blank for any)            |
| target         | option as text      | ACCEPT/REJECT/DROP                          |
| enabled        | boolean             | True/False                                  |

firewall-custom-rule attributes

| attribute name | property type       | valid values / examples                                         |
|----------------|---------------------|-----------------------------------------------------------------|
| index          | number              | optional: index of the  custom rule                             |
| name           | text                | rule name, used for file name (do not use spaces, slashes etc.) |
| content        | text                | shell script content                                            |
| reload         | boolean             | True/False                                                      |

Example Playbook
----------------

```
- role: openwrt-firewall
  zones:
  - index: 2
    name: guest
    network: guest
    forward: REJECT
    input: REJECT
    output: ACCEPT
    masq: True   
  forwardings:
  - index: 0
    src: lan
    dest: wan
  - index: 1
    src: guest
    dest: wan    
  redirects:
  - name: my_tcpudp_rule
    src: wan
    proto: tcpudp
    src_dport: 2222
    dest_ip: '192.168.1.3'
    dest_port: 22
    target: DNAT
    dest: lan
  - name: my_tcp_rule
    src: wan
    proto: tcp
    src_dport: 80
    dest_ip: '192.168.1.3'
    dest_port: 80
    target: DNAT
    dest: lan
  - name: my_udp_rule
    src: wan
    proto: udp
    src_dport: 43210
    dest_ip: '192.168.1.3'
    dest_port: 43210
    target: DNAT
    dest: lan
  rules:
  - index: 9 # attention: there are some default rules so you have to setup the index here otherwise you will overwrite them!
    name: my_custom_forward_block
    family: ipv4
    src: lan
    target: DROP
    dest_ip: '82.165.230.17'      
  - index: 10
    name: my_custom_input_block
    family: ipv4
    src: wan
    src_ip: 82.165.230.17
    dest: lan
    target: DROP
  custom_rules:
  - name: my_custom_rule
    content: "{{ lookup('file', './iptables/my_custom_iptables_script') }}"
    reload: True
```

Official documentation:
* [OpenWRT Wiki / Firewall configuration](http://wiki.openwrt.org/doc/uci/firewall)

Got idea from:
* [lefant/ansible-openwrt-firewall](https://github.com/lefant/ansible-openwrt-firewall)
