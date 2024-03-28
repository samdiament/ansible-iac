# README for `infrastructure.azure_subnet`

## Description

An Ansible Role that manages subnets in Microsoft Azure.

## Requirements

* `azure.azcollection`

## Role Variables

### Globals

Variables that are common across all Azure related roles in this collection are described in the table below.

|       Variable Name        | Type |                                               Description                                               |
|:--------------------------:|:----:|:-------------------------------------------------------------------------------------------------------:|
| `azure_resourcegroup_name` | str  | Name of resource group to use. Can be overridden on an object related basis (see `azure_subnets` below) |

### Subnet Management

Subnets are managed through the `azure_subnets` list variable.
Each element in the list supports the variables described in the table below.

|              Variable Name              | Default | Required  |   Type    |                                Description                                |
|:---------------------------------------:|:-------:|:---------:|:---------:|:-------------------------------------------------------------------------:|
|                 `name`                  |   ""    |    yes    |    str    |                              Name of subnet                               |
|                 `state`                 |   ""    |    yes    |    str    |              Desired state of subnet (`present` or `absent`)              |
|            `resource_group`             |   ""    |    no     |    str    |           Name of resource group to create/update/delete subnet           |
|          `address_prefix_cidr`          |   ""    | on create | list(str) |                   Subnet address prefix in CIDR format                    |
|         `address_prefixes_cidr`         |   ""    | on create | list(str) |                  Subnet address prefixes in CIDR format                   |
|              `nat_gateway`              |   ""    |    no     |    str    |           Name/ID of NAT gateway which to associate the subnet            |
|   `private_endpoint_network_policies`   |   ""    |    no     |    str    |   Apply network policies on private endpoints (`Enabled` or `Disabled`)   |
| `private_link_service_network_policies` |   ""    |    no     |    str    | Apply network policies on private link services (`Enabled` or `Disabled`) |
|              `route_table`              |   ""    |    no     |    str    |               Name/ID of route table to associate to subnet               |
|            `security_group`             |   ""    |    no     |    str    |             Name/ID of security group to associate to subnet              |
|           `service_endpoints`           |   ""    |    no     | list(str) |                        Array of service endpoints                         |

## Dependencies

None.

## Example Playbook

```yaml
---
- name: Ensure subnets at desired state
  hosts: localhost
  become: false

  roles:
  - role: pumphouse_p.infrastructure.azure_subnet
    vars:
      azure_subnets:
        - name: my_new_sub
          virtual_network_name: my_vnet
          resource_group: my_rg
          address_prefixes_cidr:
            - "192.168.122.0/24"
            - "192.168.222.0/24"
          state: present
        - name: my_new_sub2
          virtual_network_name: my_vnet
          resource_group: my_rg
          address_prefix_cidr: 192.168.123.0/24
          state: present
        - name: my_old_sub
          virtual_network_name: my_vnet
          resource_group: my_rg
          state: absent
```

## License

GPL

## Author Information

Devin Parrish ([GitHub](https://github.com/pumphouse-p))