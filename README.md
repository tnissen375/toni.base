toni.base
====

Primary: Ansible role to setup basic system packages, kernel modules, UFW firewall,...

Secondary: Helper which can be included to excute common tasks.

Requirements
------------

Ubuntu 20.04 / Debian 10 node with ansible SSH access.

Role Variables - defaults:
--------------------------

```bash
user_path: "/root"  # Path to user where files will be stored, usually the ansible become user or root
configure_firewall: false # UFW with SSH and fail2ban can be installed with this option. You will have to add application ports later in application roles
configure_ipv4_hostname: true # The node hosts file can be upated with the IPv4-address and hostname of the processed ansible node
configure_ipv6_hostname: false # The node hosts file can be upated with the IPv6-address and hostname of the processed ansible node

packages: # The apt packages which will be installed by default
  - curl
  - wget
  - dnsutils
  - git
  - sudo
  - nano
  - python3
  - python3-pip
  - python3-setuptools
  - haveged

packages_add: []

pippackages:
  - pyaml
  - psycopg2-binary
  - jmespath
  #- dnspython

pippackages_add: []

kernel_modules:
  - br_netfilter
  - ip6_udp_tunnel
  - ip_set 
  - ip_set_hash_ip
  - ip_set_hash_net
  - iptable_filter
  - iptable_nat
  - iptable_mangle
  - iptable_raw
  - nf_conntrack_netlink
  - nf_defrag_ipv4
  - nf_nat
  - nfnetlink
  - udp_tunnel
  - veth
  - vxlan
  - x_tables
  - xt_addrtype
  - xt_conntrack
  - xt_comment
  - xt_mark
  - xt_multiport
  - xt_nat 
  - xt_recent
  - xt_set
  - xt_statistic
  - xt_tcpudp

kernel_modules_add: []
```

Example Playbook 
----------------

```yaml
  - hosts: myHost
    remote_user: myUser
    become: True
    roles:
      - {
          role: base,
          configure_firewall: 'true',
          configure_ipv4_hostname: 'false'
        }
```

Included Task Example
---------------------
replace_after is used to replace a line after a found expression (next line) in a file. 
There is no implementation like this in lineinfile.

```yaml
- name: Secure locations
  include_role:
    name: base
    tasks_from: replace_after
  vars:
    findlist:
      - { find: "location", path: "/root/docker/build/nginx/conf.d/default.conf", insert: "include snippets/auth.conf;" }
```

License
-------

MIT

Author Information
------------------

T.Nissen