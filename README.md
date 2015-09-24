# Ansible Playbook for Deploying OpenContrail
This Playbook can run for only Ubuntu package.
## How to Use

1. Copy the ``contrail-install-packages_x.xx-xx~xxxx_all.deb`` to the directory ``install/files/``.
2. Modified to suit the ``hosts`` file to the environment.
3. Modified to suit the variable file to the environment.
  - ``group_vars/all.yml`` and files in the ``host_vars/``
4. Run ``ansible-playbook -K -i hosts setup.yml``.
  - need sudo password
5. Reboot all nodes.

## Sample Configuration

### Hosts

```
[database]
system001
system002
system003

[config]
system001
system002
system003

[control]
system001
system002
system003

[collector]
system001
system002
system003

[webui]
system001

[system]
system001
system002
system003

[tsn]
system004
system005
```

### Group Vars

Fill in all the variables and save to directory ``group_vars/`` named ``all.yml``.

```
---
ansible_ssh_user: "ssh_username_here!"
ansible_ssh_pass: "ssh_password_here!"

contrail_keystone_address: "10.84.50.100"
contrail_admin_user: "keystone_username"
contrail_admin_password: "keystone_password"

contrail_keepalived: yes
contrail_haproxy_address: "10.84.50.80" # virtual address or config node address
contrail_netmask: "255.255.255.0"
contrail_prefixlen: "24"
contrail_gateway: "10.84.50.253"

contrail_router_asn: "64512"

contrail_tor_agents:
  - name: "test01"
    address: "10.84.50.81"
    ovs_protocol: "pssl"
    ovs_port: "9991"
    tunnel_address: "10.84.50.81"
    http_server_port: "9011"
    vendor_name: "Juniper"
    product_name: "QFX5100"
    tsn_names: [ "contrail004", "contrail005" ]
  - name: "test02"
    address: "10.84.50.82"
    ovs_protocol: "pssl"
    ovs_port: "9992"
    tunnel_address: "10.84.50.82"
    http_server_port: "9012"
    vendor_name: "Juniper"
    product_name: "QFX5100"
    tsn_names: [ "contrail004", "contrail005" ]
```

### Host Vars

Fill in all the variables and save to directory ``host_vars/`` named ``<hostname>.yml``.

```
---
contrail_device: "p4p1"
contrail_address: "10.84.50.1"
contrail_mgmt_address: "172.27.113.85"
```
