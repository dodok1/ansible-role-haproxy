# Ansible Role: HAProxy

[![Build Status](https://travis-ci.org/geerlingguy/ansible-role-haproxy.svg?branch=master)](https://travis-ci.org/geerlingguy/ansible-role-haproxy)

Installs HAProxy on RedHat/CentOS and Debian/Ubuntu Linux servers.

**Note**: This role _officially_ supports HAProxy versions 1.4 or 1.5. Future versions may require some rework.

## Requirements

None.

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

    haproxy_socket: /var/lib/haproxy/stats

The socket through which HAProxy can communicate (for admin purposes or statistics). To disable/remove this directive, set `haproxy_socket: ''` (an empty string).

    haproxy_chroot: /var/lib/haproxy

The jail directory where chroot() will be performed before dropping privileges. To disable/remove this directive, set `haproxy_chroot: ''` (an empty string). Only change this if you know what you're doing!

    haproxy_user: haproxy
    haproxy_group: haproxy

The user and group under which HAProxy should run. Only change this if you know what you're doing!

    haproxy_frontend_name: 'http-frontend'
    haproxy_frontend_bind_address: '*'
    haproxy_frontend_port: 80
    haproxy_frontend_mode: 'http'
    haproxy_http_redirect: 'true'

HAProxy HTTP frontend configuration directives. If  haproxy_http_redirect is true, requests will beredirected to https.

    haproxy_https_frontend_enabled: true
    haproxy_https_frontend_name: 'https-frontend'
    haproxy_https_frontend_bind_address: '*'
    haproxy_https_frontend_port: 443
    haproxy_https_frontend_mode: 'http'
    haproxy_ssl_certificate: '/etc/ssl/certs/example.com.pem'

HAProxy HTTPS frontend configuration directives. If haproxy_https_frontend_enabled is true.
		
    haproxy_backend_default: true
    haproxy_backend_name: 'habackend'
    haproxy_backend_mode: 'http'
    haproxy_backend_balance_method: 'roundrobin'
    haproxy_backend_httpchk: 'HEAD / HTTP/1.1\r\nHost:localhost'

HAProxy backend configuration directives. If haproxy_backend_default is true, backend will be as this.

    haproxy_contexts_enabled: true
    haproxy_contexts:
     - { name: 'app1', primary_ip: '10.10.10.1', backup_ip: '10.10.10.2', port: '8080' }

HAProxy acls with apropriate backend will be created for defined contexts. If haproxy_contexts_enabled is true.

    haproxy_backend_servers:
      - name: app1
        address: 192.168.0.1:80
      - name: app2
        address: 192.168.0.2:80

HAProxy will proxy /  to "backend:/foo"

    haproxy_default_context: 'foo'

HAProxy optional service definitions.

    optional_service: 
     - enabled: true
     - name: sshd
     - port: 7999
     - backend_ip: 10.33.179.143


A list of backend servers (name and address) to which HAProxy will distribute requests.

    haproxy_global_vars:
      - 'ssl-default-bind-ciphers ABCD+KLMJ:...'
      - 'ssl-default-bind-options no-sslv3'

A list of extra global variables to add to the global configuration section inside `haproxy.cfg`.

## Dependencies

None.

## Example Playbook

    - hosts: balancer
      sudo: yes
      roles:
        - { role: geerlingguy.haproxy }

## License

MIT / BSD

## Author Information

This role was created in 2015 by [Jeff Geerling](http://www.jeffgeerling.com/), author of [Ansible for DevOps](https://www.ansiblefordevops.com/).
