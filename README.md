openwrt-firewall
================

configure firewall aspects of your openwrt system.
compare: [http://wiki.openwrt.org/doc/uci/firewall]

Role Variables
--------------

| variable name     | type             | element structure    | default |
|-------------------|------------------|----------------------|---------|
| zones             | array of objects | see attributes below | <empty> |
| forwardings       | array of objects | see attributes below | <empty> |

Role Variable elements
----------------------

firewall-zone attributes:

| attribute name | property type       | valid values / examples            |
|----------------|---------------------|------------------------------------|
| name           | text                | zone name like 'zone-1' or 'guest' |
| network        | text                | network name this zone belongs to  |
| forward        | option as text      | ACCEPT/DROP/REJECT                 |
| input          | option as text      | ACCEPT/DROP/REJECT                 |
| output         | option as text      | ACCEPT/DROP/REJECT                 |
| masq           | boolean             | True/False                         |

firewall-forwarding attributes:

| attribute name | property type       | valid values / examples            |
|----------------|---------------------|------------------------------------|
| name           | text                | zone name like 'guest_forwarding ' |
| src            | text                | source network name                |
| dest           | text                | destination network name           |

Dependencies
------------

* openwrt-uci

Example Playbook
----------------

```
- role: openwrt-firewall
  firewall_zones: [{
    name: guest,
    network: guest,
    forward: ACCEPT,
    input: ACCEPT,
    output: ACCEPT,
    masq: True
  }]
  forwardings: [{
    src: lan,
    dest: wan
  }, {
    src: guest,
    dest: wan
  }]
```

[http://wiki.openwrt.org/doc/uci/wireless]: http://wiki.openwrt.org/doc/uci/firewall
[https://github.com/lefant/ansible-openwrt-firewall]: https://github.com/lefant/ansible-openwrt-firewall
