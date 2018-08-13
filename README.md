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
  left:
    address: "%any"
  right:
    address: "%any"

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
    secret: test
```

## Example Playbook

IKEv1:
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
            credentials:
              - type: PSK
                secret: test
```

IKEv2:
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
              keyexchange: ikev2
            left:
              address: 1.2.3.4
              subnet: 0.0.0.0/0
              cert: server-cert.pem
              firewall: "yes"
            right:
              auth: eap-radius
            credentials:
              - type: RSA
                secret: server-key.pem
        plugins:
          - filename: eap-radius.conf
            config:
              block: |
                  server-0 {
                      address = 127.0.0.1
                      secret = test
                  }
              insertafter: 'servers \{'
```

Certificates and keys have to be provided manually in:
* /etc/ipsec.d/certs/
* /etc/ipsec.d/private/

## License

GPLv2