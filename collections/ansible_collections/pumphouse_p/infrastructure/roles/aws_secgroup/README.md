# README for `infrastructure.aws_secgroup`

## Description

An Ansible Role to manage security groups and security group rules in AWS.

## Requirements

- `amazon.aws`

## Role Variables

### General Configuration

|    Variable Name     |  Default  | Required | Type |       Description       |
|:--------------------:|:---------:|:--------:|:----:|:-----------------------:|
| `aws_resource_state` | `present` |    no    | str  | Desired state of subnet |
|     `aws_region`     |    ""     |   yes    | str  |  The AWS region to use  |

### Security Group Configuration

The following variables are available when `aws_resource_state == 'present'`.

|        Variable Name         | Default | Required |     Type     |                            Description                            |
|:----------------------------:|:-------:|:--------:|:------------:|:-----------------------------------------------------------------:|
|     `aws_secgroup_name`      |   ""    |   yes    |     str      |               Name to assign to the security group                |
|    `aws_secgroup_vpc_id`     |   ""    |   yes    |     str      |             The ID of the VPC to create the group in              |
|  `aws_secgroup_description`  |   ""    |   yes    |     str      |                 Description of the security group                 |
|  `aws_secgroup_purge_tags`   | `true`  |    no    |     bool     |     Whether to remove tags that are not provided to the role      |
| `aws_secgroup_purge_ingress` | `true`  |    no    |     bool     | Whether to remove ingress rules that are not provided to the role |
| `aws_secgroup_purge_egress`  | `true`  |    no    |     bool     | Whether to remove egress rules that are not provided to the role  |
| `aws_secgroup_ingress_rules` |  `[]`   |    no    |  list(map)   |         List of ingress rules. See below for more details         |
| `aws_secgroup_egress_rules`  |  `[]`   |    no    |  list(map)   |         List of egress rules. See below for more details          |
|   `aws_secgroup_user_tags`   |  `{}`   |    no    | map(str:str) |              Custom tags to apply to security group               |

The following variables are available when `aws_resource_state == 'absent'`.

|   Variable Name   | Default | Required | Type |            Description             |
|:-----------------:|:-------:|:--------:|:----:|:----------------------------------:|
| `aws_secgroup_id` |   ""    |   yes    | str  | ID of the security group to delete |

### Security Group Rules Configuration

The following variables are available for ingress rules defined as items in the `aws_secgroup_ingress_rules` list.

| Variable Name | Default | Required |    Type     |                         Description                         |
|:-------------:|:-------:|:--------:|:-----------:|:-----------------------------------------------------------:|
|   `cidr_ip`   |   ""    |    no    | `cidrblock` |         The IPv4 CIDR range traffic is coming from          |
|  `cidr_ipv6`  |   ""    |    no    | `cidrblock` |         The IPv6 CIDR range traffic is coming from          |
|  `from_port`  |   ""    |    no    |     int     | The start of the range of ports that traffic is coming from |
|   `to_port`   |   ""    |    no    |     str     |  The end of the range of ports that traffic is coming from  |
|  `group_id`   |   ""    |    no    |     str     |     The ID of the security group traffic is coming from     |
| `group_name`  |   ""    |    no    |     str     |    The name of the security group traffic is coming from    |
|  `ip_prefix`  |   ""    |    no    |     str     |            The IP Prefix traffic is coming from             |
|    `proto`    |   ""    |    no    |     str     |   The IP protocol (`tcp`, `udp`, `icmp`, `icmpv6`) in use   |
|  `rule_desc`  |   ""    |    no    |     str     |                 A description for the rule                  |

The following variables are available for egress rules defined as items in the `aws_secgroup_egress_rules` list.

| Variable Name | Default | Required |    Type     |                       Description                        |
|:-------------:|:-------:|:--------:|:-----------:|:--------------------------------------------------------:|
|   `cidr_ip`   |   ""    |    no    | `cidrblock` |         The IPv4 CIDR range traffic is going to          |
|  `cidr_ipv6`  |   ""    |    no    | `cidrblock` |         The IPv6 CIDR range traffic is going to          |
|  `from_port`  |   ""    |    no    |     int     | The start of the range of ports that traffic is going to |
|   `to_port`   |   ""    |    no    |     str     |  The end of the range of ports that traffic is going to  |
|  `group_id`   |   ""    |    no    |     str     |     The ID of the security group traffic is going to     |
| `group_name`  |   ""    |    no    |     str     |    The name of the security group traffic is going to    |
|  `ip_prefix`  |   ""    |    no    |     str     |            The IP Prefix traffic is going to             |
|    `proto`    |   ""    |    no    |     str     | The IP protocol (`tcp`, `udp`, `icmp`, `icmpv6`) in use  |
|  `rule_desc`  |   ""    |    no    |     str     |                A description for the rule                |


## Dependencies

None.

## Example Playbook

```yaml
---
- name: Playbook to create a security group on AWS
  hosts: localhost
  gather_facts: false
  become: false

  roles:
    - role: pumphouse_p.infrastructure.aws_secgroup
      aws_region: us-east-2
      aws_resource_state: present
      aws_secgroup_name: ssh
      aws_secgroup_vpc_id: vpc-12345
      aws_secgroup_description: Allow inbound SSH traffic
      aws_secgroup_ingress_rules:
        - cidr_ip: 0.0.0.0/0
          from_port: 22
          to_port: 22
          proto: tcp
          rule_desc: Allow inbound traffic on tcp/22
      aws_secgroup_user_tags:
        provider: aws
```

## License

GPL

## Author Information

Devin Parrish ([GitHub](https://github.com/pumphouse-p))