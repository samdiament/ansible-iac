# README for `infrastructure.azure_network`

## Description

An Ansible Role that manages virtual networks in Microsoft Azure.

## Requirements

* `azure.azcollection`

## Role Variables

### Globals

Variables that are common across all Azure related roles in this collection are described in the table below.

|       Variable Name        | Type |                                               Description                                                |
|:--------------------------:|:----:|:--------------------------------------------------------------------------------------------------------:|
| `azure_resourcegroup_name` | str  | Name of resource group to use. Can be overridden on an object related basis (see `azure_networks` below) |

### Defaults

Defaults are set in the `defaults/main.yml` file and described in the table below.

|             Variable Name              |                   Default                    | Type |                          Description                          |
|:--------------------------------------:|:--------------------------------------------:|:----:|:-------------------------------------------------------------:|
|      `azure_network_append_tags`       |                    `true`                    | bool | Whether tags should be appended to existing or overwrite them |
| `azure_network_purge_address_prefixes` |                   `false`                    | bool |        Overwrite existing address prefixes if present         |
|   `azure_network_purge_dns_servers`    |                   `false`                    | bool |           Overwrite existing DNS servers if present           |
|      `azure_network_common_tags`       | `{managed_by: ansible, created_by: ansible}` | dict |    Common tags which should be present on storage accounts    |

### Virtual Network Management

Virtual networks are managed through the `azure_networks` list variable.
Each element in the list supports the variables described in the table below.

|      Variable Name       | Default | Required  |   Type    |                          Description                           |
|:------------------------:|:-------:|:---------:|:---------:|:--------------------------------------------------------------:|
|          `name`          |   ""    |    yes    |    str    |                    Name of virtual network                     |
|         `state`          |   ""    |    yes    |    str    |    Desired state of virtual network (`present` or `absent`)    |
|     `resource_group`     |   ""    |    no     |    str    | Name of resource group to create/update/delete virtual network |
| `address_prefixes_cidr`  |   ""    | on create | list(str) |                Network prefixes in CIDR format                 |
|      `append_tags`       | `true`  |    no     |   bool    | Whether tags should be appended to existing or overwrite them  |
|      `dns_servers`       |   ""    |    no     | list(str) |                   DNS servers to use (max 2)                   |
|        `location`        |   ""    |    no     |    str    |             Location (defaults to resource group)              |
| `purge_address_prefixes` | `false` |    no     |   bool    |         Overwrite existing address prefixes if present         |
|   `purge_dns_servers`    | `false` |    no     |   bool    |           Overwrite existing DNS servers if present            |
|          `tags`          |   ""    |    no     |   dict    |                 Tags to set on virtual network                 |

## Dependencies

None.

## Example Playbook

```yaml
---
- name: Ensure virutal networks at desired state
  hosts: localhost
  become: false

  roles:
  - role: pumphouse_p.infrastructure.azure_network
    vars:
      azure_networks:
        - name: core_network
          resource_group: my_rg
          address_prefixes_cidr:
            - "192.168.122.0/24"
            - "192.168.222.0/24"
          dns_servers:
            - "8.8.8.8"
            - "8.8.4.4"
          state: present
        - name: legacy_network
          resource_group: my_rg
          state: absent        
```

## License

GPL

## Author Information

Devin Parrish ([GitHub](https://github.com/pumphouse-p))