# README for `infrastructure.aws_vpc`

## Description

An Ansible Role to create a VPC on AWS.

## Requirements

- `amazon.aws`

## Role Variables

### General Configuration

|    Variable Name     |  Default  | Required | Type |      Description      |
|:--------------------:|:---------:|:--------:|:----:|:---------------------:|
| `aws_resource_state` | `present` |    no    | str  | Desired state of VPC  |
|     `aws_region`     |    ""     |   yes    | str  | The AWS region to use |

### VPC Configuration

|      Variable Name      |     Default     | Required |     Type     |                           Description                           |
|:-----------------------:|:---------------:|:--------:|:------------:|:---------------------------------------------------------------:|
|     `aws_vpc_name`      |       ""        |   yes    |     str      |                           Name of VPC                           |
|  `aws_vpc_cidr_block`   | `172.16.0.0/16` |    no    | `cidrblock`  |                   The primary CIDR of the VPC                   |
| `aws_vpc_dns_hostnames` |     `true`      |    no    |     bool     |             Whether to enable AWS hostname support              |
|  `aws_vpc_dns_support`  |     `true`      |    no    |     bool     |                Whether to enable AWS DNS support                |
|  `aws_vpc_purge_cidrs`  |     `true`      |    no    |     bool     | Remove existing CIDRs are not specified in `aws_vpc_cidr_block` |
|   `aws_vpc_ipv6_cidr`   |     `false`     |    no    |     bool     |               Request an IPv6 CIDR block (`/56`)                |
|   `aws_vpc_user_tags`   |      `{}`       |    no    | map(str:str) |                   Custom tags to apply to VPC                   |

## Dependencies

None.

## Example Playbook

```yaml
---
- name: Playbook to create VPC on AWS
  hosts: localhost
  gather_facts: false
  become: false

  roles:
    - role: pumphouse_p.infrastructure.aws_vpc
      aws_region: us-east-2
      aws_resource_state: present
      aws_vpc_name: my_vpc
      aws_vpc_cidr_block: 192.168.0.0/16
      aws_vpc_user_tags:
        provider: aws
        ipv6_enabled: "false"
```

## License

GPL

## Author Information

Devin Parrish ([GitHub](https://github.com/pumphouse-p))