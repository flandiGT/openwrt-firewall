openwrt-firewall
================

configure firewall aspects of your openwrt system.
compare: [http://wiki.openwrt.org/doc/uci/firewall]

Role Variables
--------------

| variable name     | type             | element structure    | default |
|-------------------|------------------|----------------------|---------|
| firewall_zones    | array of objects | see attributes below | <empty> |

Role Variable elements
----------------------

firewall-zone attributes:

| attribute name | property type       | valid values / examples                                                      |
|----------------|---------------------|------------------------------------|
| name           | text                | zone name line 'zone-1' or 'guest' |
| network        | text                | network name this zone belongs to  |
| forward        | option as text      | ACCEPT/DROP/REJECT                 |
| input          | option as text      | ACCEPT/DROP/REJECT                 |
| output         | option as text      | ACCEPT/DROP/REJECT                 |

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
    output: ACCEPT
  }]
```

[http://wiki.openwrt.org/doc/uci/wireless]: http://wiki.openwrt.org/doc/uci/firewall
[https://github.com/lefant/ansible-openwrt-firewall]: https://github.com/lefant/ansible-openwrt-firewall
