# README for `infrastructure.aws_keypair`

## Description

An Ansible Role to create a keypair on AWS.

## Requirements

- `amazon.aws`

## Role Variables

### General Configuration

|    Variable Name     |  Default  | Required | Type |       Description       |
|:--------------------:|:---------:|:--------:|:----:|:-----------------------:|
| `aws_resource_state` | `present` |    no    | str  | Desired state of subnet |
|     `aws_region`     |    ""     |   yes    | str  |  The AWS region to use  |

### Keypair Configuration

|      Variable Name       | Default | Required |     Type     |                       Description                        |
|:------------------------:|:-------:|:--------:|:------------:|:--------------------------------------------------------:|
|    `aws_keypair_name`    |   ""    |   yes    |     str      |                 The name of the keypair                  |
| `aws_keypair_overwrite`  | `false` |    no    |     bool     |            Overwrite key if it already exists            |
|   `aws_keypair_pubkey`   |   ""    |    no    |     str      |                  Contents of public key                  |
| `aws_keypair_purge_tags` | `true`  |    no    |     bool     | Whether to remove tags that are not provided to the role |
| `aws_keypair_user_tags`  |  `{}`   |    no    | map(str:str) |             Custom tags to apply to keypair              |

## Dependencies

None.

## Example Playbook

```yaml
---
- name: Playbook to create a keypair on AWS
  hosts: localhost
  gather_facts: false
  become: false

  roles:
    - role: pumphouse_p.infrastructure.aws_keypair
      aws_region: us-east-2
      aws_keypair_name: id_rsa
      aws_keypair_pubkey: "{{ lookup('file', '~/.ssh/id_rsa.pub') }}"
      aws_keypair_user_tags:
        client: laptop
```

## License

GPL

## Author Information

Devin Parrish ([GitHub](https://github.com/pumphouse-p))