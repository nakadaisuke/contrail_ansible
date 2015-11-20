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
All of Hosts must be resolved its IP address by DNS or hosts
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
system002
system003

[compute]
system004
system005

[tsn]
system006
system007
```

### Group Vars

Fill in all the variables and save to directory ``group_vars/`` named ``all.yml``.

```
---
# set install package in /install/files/
package: "contrail-install-packages_2.21-102~juno_all.deb"
# set "yes" if you want to upgrade kernel to recommendation version.
kernel_install: yes 
ansible_ssh_user: "ssh_username_here!" 
ansible_ssh_pass: "ssh_password_here!"

contrail_keystone_address: "keystone IP address"
contrail_admin_user: "keystone_username"
contrail_admin_password: "keystone_password"

# set "yes" if you want to use Virtual IP address for redundancy
contrail_keepalived: no 
contrail_haproxy_address: "10.0.0.22/24" # Virtual IP address

contrail_router_asn: "64512" # AS number of this system
rabbit_password: "password" # rabbitMQ password

# set "yes" if you want to modify keystone endpoint 
keystone_provision: no
# set "yes" if you want to install nova-compute 
install_nova: yes

rabbit_password: "password" # rabbitMQ password

# modify tor-agent value
contrail_tor_agents:
  - name: "test01"
    address: "10.0.0.81"
    ovs_protocol: "pssl"
    ovs_port: "9991"
    tunnel_address: "10.0.0.81"
    http_server_port: "9011"
    vendor_name: "Juniper"
    product_name: "QFX5100"
    tsn_names: [ "system006" , "system007"]
  - name: "test02"
    address: "10.0.0.82"
    ovs_protocol: "pssl"
    ovs_port: "9992"
    tunnel_address: "10.0.0.82"
    http_server_port: "9012"
    vendor_name: "Juniper"
    product_name: "QFX5100"
    tsn_names: [ "system006" , "system007"]
```

### Host Vars

Fill in all the variables and save to directory ``host_vars/`` named ``<hostname>.yml``.

```
---
contrail_device: "p4p1"  # Control/Data plane interface
contrail_address: "10.84.50.7"  # Control/Data plane IP address
contrail_netmask: "255.255.255.0"  # Control/Data plane subnet mask
contrail_gateway: "10.84.50.254"  # Default gateway of vRouter
contrail_mgmt_address: "172.27.113.91"  # Management interface IP address
contrail_route: [{ ip: "0.0.0.0/0", gw: "10.0.0.1", device: "vhost0" },{ ip: "10.0.0.0/0", gw: "10.0.0.1", device: "vhost0" }] # set static route
```
