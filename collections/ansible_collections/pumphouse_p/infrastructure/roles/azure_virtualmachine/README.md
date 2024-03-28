# README for `infrastructure.azure_virtualmachine`

## Description

An Ansible Role to create a virtual machine on Microsoft Azure.

## Requirements

- `azure.azcollection`

## Role Variables

### General Configuration

|       Variable Name        |         Default         | Required |     Type     |                     Description                     |
|:--------------------------:|:-----------------------:|:--------:|:------------:|:---------------------------------------------------:|
| `azure_resourcegroup_name` |           ""            |   yes    |     str      |               Name of resource group                |
|   `azure_resource_state`   |        `present`        |    no    |     str      |             Desired state of resources              |
|   `azure_resource_tags`    | `{managed_by: ansible}` |    no    | map(str:str) | Tags to apply to all resources created by this role |

### Virtual Network Configuration

|                Variable Name                 |               Default               | Required |     Type     |              Description              |
|:--------------------------------------------:|:-----------------------------------:|:--------:|:------------:|:-------------------------------------:|
|         `azure_virtualnetwork_name`          |             `core_net`              |    no    |     str      |        Name of virtual network        |
| `azure_virtualnetwork_address_prefixes_cidr` |           `172.16.0.0/16`           |    no    |  list(str)   | Network addresses for virtual network |
|      `azure_virtualnetwork_dns_servers`      |                 ""                  |    no    |  list(str)   |       Custom DNS servers to use       |
|         `azure_virtualnetwork_tags`          | `{Name: azure_virtualnetwork_name}` |    no    | map(str:str) |             Tags to apply             |

### Subnet Configuration

Set subnet properties under the `azure_subnets` list. Each element in the list supports the variables below. Subnets will be associated with the Virtual Network defined above.

|     Variable Name     |     Default     | Required |     Type     |        Description         |
|:---------------------:|:---------------:|:--------:|:------------:|:--------------------------:|
|        `name`         |  `core_subnet`  |    no    |     str      |       Name of subnet       |
| `address_prefix_cidr` | `172.16.0.0/24` |    no    |     str      | Network address for subnet |

### Security Group Configuration

Security group that will be associated with the subnet above.

|        Variable Name         |         Default         | Required |     Type     |      Description       |
|:----------------------------:|:-----------------------:|:--------:|:------------:|:----------------------:|
|  `azure_securitygroup_name`  |        `default`        |    no    |     str      | Name of security group |
| `azure_security_group_rules` | see `defaults/main.yml` |    no    |  list(map)   |     List of rules      |
| `azure_security_group_tags`  |     `{Name: name}`      |    no    | map(str:str) |     Tags to apply      |

## Dependencies

None.

## Example Playbook

```yaml
---
- name: Playbook to create core network infrastructure on Azure
  hosts: localhost
  gather_facts: false
  become: false
  vars:
    azure_resourcegroup_name: myazure_rg

  roles:
    - pumphouse_p.infrastructure.azure_core_network
```

## License

GPL

## Author Information

Devin Parrish ([GitHub](https://github.com/pumphouse-p))