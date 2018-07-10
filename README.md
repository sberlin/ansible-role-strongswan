# Ansible strongSwan Role

An Ansible role for the configuration of strongSwan with support for Ubuntu.

## Requirements

## Role Variables

```YAML
# defaults/main.yml
strongswan_conn_default:
  auto: add
  authby: psk
  keyexchange: ikev2
  dpdaction: restart
  dpddelay: 30

strongswan_conn: []

strongswan_charondebug: "cfg 2, dmn 2, ike 2, net 0"
```

Connection information to be installed into strongSwan:

```YAML
strongswan_conn:
  - name: dpt-1-server
    conn:
      auto: add
      type: tunnel
      authby: psk
      keyexchange: ikev1
    left:
      address: 1.2.3.4
      firewall: "yes"
      hostaccess: "yes"
    right:
      address: "%any"
      sourceip: 10.0.0.0/24
    secret: test
```

## Example Playbook

```YAML
- hosts: servers
  roles:
    - role: strongswan
      vars:
        strongswan_conn:
          - name: dpt-1-server
            conn:
              auto: add
              type: tunnel
              authby: psk
              keyexchange: ikev1
            left:
              address: 1.2.3.4
              firewall: "yes"
              hostaccess: "yes"
            right:
              address: "%any"
              sourceip: 10.0.0.0/24
            secret: test
```

## License

GPLv2