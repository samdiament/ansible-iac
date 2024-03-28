# README for `infrastructure.azure_securitygroup`

## Description

An Ansible Role that manages security groups and rules on Microsoft Azure.

## Requirements

* `azure.azcollection`

## Role Variables

### Defaults

|               Variable Name               | Default | Type |                           Description                           |
|:-----------------------------------------:|:-------:|:----:|:---------------------------------------------------------------:|
|     `azure_securitygroup_append_tags`     | `true`  | bool | Whether tags should be added to existing or overwrite entirely  |
| `azure_securitygroup_purge_default_rules` | `false` | bool |          Whether default rules should be purged or not          |
|     `azure_securitygroup_purge_rules`     | `false` | bool | Whether rules should be added to existing or overwrite entirely |

### Security Group management

Security groups are managed through the `azure_securitygroups` list variable. Each element in the
list supports the variables described in the table below.

|     Variable Name     |                  Default                  | Required |    Type    |                          Description                           |
|:---------------------:|:-----------------------------------------:|:--------:|:----------:|:--------------------------------------------------------------:|
|        `name`         |                    ""                     |   yes    |    str     |                     Name of security group                     |
|   `resource_group`    |        `azure_resourcegroup_name`         |   yes    |    str     |                     Name of resource group                     |
|        `state`        |                    ""                     |   yes    |    str     |             Desired state (`present` or `absent`)              |
|     `append_tags`     |     `azure_securitygroup_append_tags`     |    no    |    bool    | Whether tags should be added to existing or overwrite entirely |
|    `default_rules`    |                    ""                     |    no    | list(dict) |           Set of default rules to shape traffic flow           |
|      `location`       |                    ""                     |    no    |    str     |     Azure location. Defaults to location of resource group     |
| `purge_default_rules` | `azure_securitygroup_purge_default_rules` |    no    |    bool    |       Remove existing rules not matching those provided        |
|     `purge_rules`     |     `azure_securitygroup_purge_rules`     |    no    |    bool    |       Remove existing rules not matching those provided        |
|        `rules`        |                    ""                     |    no    | list(dict) |                          Set of rules                          |


## Dependencies

None.

## Example Playbook

```yaml
---
- name: Manage Azure security groups
  hosts: localhost
  become: false

  roles:
  - role: pumphouse_p.infrastructure.azure_securitygroup
    vars:
      azure_securitygroup_append_tags: false
      azure_resourcegroup_name: my_rg
      azure_securitygroups:
        - name: ssh
          state: present
          rules:
            - name: AllowSSH
              protocol: Tcp
              source_address_prefix:
                - "0.0.0.0/0"
              destination_port_range: 22
              access: Allow
              priority: 101
              direction: Inbound
        - name: ftp
          state: absent
```

## License

GPL

## Author Information

Devin Parrish ([GitHub](https://github.com/pumphouse-p))