# ansible-role-haproxy

[![Latest](https://github.com/noveris-inf/ansible-role-haproxy/workflows/Latest/badge.svg)](https://github.com/noveris-inf/ansible-role-haproxy/actions?query=workflow%3ALatest) [![Versioned](https://github.com/noveris-inf/ansible-role-haproxy/workflows/Versioned/badge.svg)](https://github.com/noveris-inf/ansible-role-haproxy/actions?query=workflow%3AVersioned)


Example:

```
    - role: haproxy
      vars:
        haproxy_configuration: |
          # /etc/haproxy/haproxy.cfg
          #---------------------------------------------------------------------
          # Global settings
          #---------------------------------------------------------------------
          global
              log /dev/log local0
              log /dev/log local1 notice
              daemon

          #---------------------------------------------------------------------
          # common defaults that all the 'listen' and 'backend' sections will
          # use if not designated in their block
          #---------------------------------------------------------------------
          defaults
              mode                    http
              log                     global
              option                  httplog
              option                  dontlognull
              option http-server-close
              option forwardfor       except 127.0.0.0/8
              option                  redispatch
              retries                 1
              timeout http-request    10s
              timeout queue           20s
              timeout connect         5s
              timeout client          20s
              timeout server          20s
              timeout http-keep-alive 10s
              timeout check           10s

          #---------------------------------------------------------------------
          # frontend01
          #---------------------------------------------------------------------
          frontend frontend01
              bind 192.168.1.10:20100
              mode tcp
              option tcplog
              default_backend backend01

          backend backend01
              option httpchk GET /health-uri
              http-check expect status 200
              mode tcp
              balance     roundrobin
              server server01 192.168.2.100:443 check verify none inter 2000
              server server02 192.168.2.101:443 check verify none inter 2000
              server server03 192.168.2.102:443 check verify none inter 2000
```
