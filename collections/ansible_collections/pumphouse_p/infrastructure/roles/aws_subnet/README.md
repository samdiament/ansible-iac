# README for `infrastructure.aws_subnet`

## Description

An Ansible Role to create a subnet on AWS.

## Requirements

- `amazon.aws`

## Role Variables

### General Configuration

|    Variable Name     |  Default  | Required | Type |       Description       |
|:--------------------:|:---------:|:--------:|:----:|:-----------------------:|
| `aws_resource_state` | `present` |    no    | str  | Desired state of subnet |
|     `aws_region`     |    ""     |   yes    | str  |  The AWS region to use  |

### Subnet Configuration

|           Variable Name            |     Default     | Required |     Type     |                       Description                        |
|:----------------------------------:|:---------------:|:--------:|:------------:|:--------------------------------------------------------:|
|         `aws_subnet_cidr`          | `172.16.0.0/24` |    no    | `cidrblock`  |              The CIDR block for the subnet               |
|        `aws_subnet_vpc_id`         |       ""        |   yes    |     str      |      VPC ID in which to create or delete the subnet      |
|      `aws_subnet_map_public`       |     `false`     |    no    |     bool     |     Whether to assign public IP addresses by default     |
|      `aws_subnet_purge_tags`       |     `true`      |    no    |     bool     | Whether to remove tags that are not provided to the role |
|         `aws_subnet_name`          |       ""        |    no    |     str      |           Value of Name tag to apply to subnet           |
|       `aws_subnet_user_tags`       |      `{}`       |    no    | map(str:str) |              Custom tags to apply to subnet              |
| `aws_subnet_assign_instances_ipv6` |     `false`     |    no    |     bool     |           Automatically assign IPv6 addresses            |
|          `aws_subnet_az`           |       ""        |    no    |     str      |           The availability zone for the subnet           |
|       `aws_subnet_ipv6_cidr`       |       ""        |    no    | `cidrblock`  |            The IPv6 CIDR block for the subnet            |
|         `aws_subnet_wait`          |     `true`      |    no    |     bool     |    Wait for subnet to be available before continuing     |
|     `aws_subnet_wait_timeout`      |      `300`      |    no    |     int      | Number of seconds to wait for subnet to become available |

## Dependencies

None.

## Example Playbook

```yaml
---
- name: Playbook to create subnet on AWS
  hosts: localhost
  gather_facts: false
  become: false

  roles:
    - role: pumphouse_p.infrastructure.aws_subnet
      aws_region: us-east-2
      aws_resource_state: present
      aws_subnet_cidr: 192.168.0.0/24
      aws_subnet_vpc_id: vpc-12345
      aws_subnet_name: my_vpc-subnet
      aws_subnet_user_tags:
        provider: aws
        ipv6_enabled: "false"
```

## License

GPL

## Author Information

Devin Parrish ([GitHub](https://github.com/pumphouse-p))