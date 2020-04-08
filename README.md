# Ansible role: AWS VPC

This role handles the creation of AWS VPC's

[![Build Status](https://travis-ci.org/Flaconi/ansible-role-aws-ec2-security-group.svg?branch=master)](https://travis-ci.org/Flaconi/ansible-role-aws-ec2-security-group)
[![Version](https://img.shields.io/github/tag/Flaconi/ansible-role-aws-ec2-security-group.svg)](https://github.com/Flaconi/ansible-role-aws-ec2-security-group/tags)
[![Ansible Galaxy](https://img.shields.io/ansible/role/d/25919.svg)](https://galaxy.ansible.com/Flaconi/aws-vpc/)

## Requirements

* Ansible 2.5


## Additional variables

Additional variables that can be used (either as `host_vars`/`group_vars` or via command line args):

| Variable                                            | Description                  |
|-----------------------------------------------------|------------------------------|
| `aws_ec2_security_group_profile`                    | Boto profile name to be used |
| `aws_ec2_security_group_default_region`             | Default region to use        |
| `aws_ec2_security_group_vpc_filter_additional`      | Additional `key` `val` filter to add to `vpc_filter` and `vpc_name` by default. |


## Example definition

#### Required parameter only

```yml
aws_ec2_security_groups:

  # Create Security Group for by VPC name
  - vpc_name: devops-test-vpc
    security_groups:
      - name: devops-test-sg
        rules:
          - proto: tcp
            ports:
              - 80
              - 443
            cidr_ip: 0.0.0.0/0

  # Create route tables for a VPC by VPC filter
  - vpc_filter:
      - key: "tag:Name"
        val: "devops-test-vpc"
      - key: "tag:env"
        val: playground
      - key: "tag:department"
        val: devops
    security_groups:
      - name: devops-test-sg-2
        rules:
          - proto: tcp
            ports: 22
            cidr_ip: 0.0.0.0/0
          # Example for NAT GW by name
          - proto: tcp
            ports: 80
            cidr_ip: 0.0.0.0/0
          # Example for NAT GW by filter
          - proto: tcp
            ports:
              - 80
              - 443
            cidr_ip: 0.0.0.0/0
          # Example for Instance by name
          - proto: tcp
            ports: 3306
            cidr_ip: 0.0.0.0/0
          # Example for Instance by filter
          - proto: tcp
            ports:
              - 80
              - 443
            cidr_ip: 0.0.0.0/0
          # Example for ENI by name
          - proto: tcp
            ports:
              - 80
              - 443
            cidr_ip: 0.0.0.0/0
          # Example for ENI by filter
          - proto: tcp
            ports:
              - 80
              - 443
            cidr_ip: 0.0.0.0/0
          # Example for VPC Peering by name
          - proto: tcp
            ports:
              - 80
              - 443
            cidr_ip: 0.0.0.0/0
          # Example for VPC Peering by filter
          - proto: tcp
            ports:
              - 80
              - 443
            cidr_ip: 0.0.0.0/0
          # Example for VGW by filter without route propagation
          - proto: tcp
            ports:
              - 80
              - 443
            cidr_ip: 0.0.0.0/0
          # Example for VGW by filter with route propagation
          - proto: tcp
            ports:
              - 80
              - 443
            cidr_ip: 0.0.0.0/0
```

#### All available parameter
```yml

aws_ec2_security_group_vpc_filter_additional:
  - key: state
    val: available

aws_ec2_security_groups:

  # Create Security Group for by VPC name
  - vpc_name: devops-test-vpc
    security_groups:
      - name: devops-test-sg
        rules:
          - proto: tcp
            ports:
              - 80
              - 443
            cidr_ip: 0.0.0.0/0

  # Create route tables for a VPC by VPC filter
  - vpc_filter:
      - key: "tag:Name"
        val: "devops-test-vpc"
      - key: "tag:env"
        val: playground
      - key: "tag:department"
        val: devops
    security_groups:
      - name: devops-test-sg-2
        rules:
          - proto: tcp
            ports: 22
            cidr_ip: 0.0.0.0/0
          # Example for NAT GW by name
          - proto: tcp
            ports: 80
            cidr_ip: 0.0.0.0/0
          # Example for NAT GW by filter
          - proto: tcp
            ports:
              - 80
              - 443
            cidr_ip: 0.0.0.0/0
          # Example for Instance by name
          - proto: tcp
            ports: 3306
            cidr_ip: 0.0.0.0/0
          # Example for Instance by filter
          - proto: tcp
            ports:
              - 80
              - 443
            cidr_ip: 0.0.0.0/0
          # Example for ENI by name
          - proto: tcp
            ports:
              - 80
              - 443
            cidr_ip: 0.0.0.0/0
          # Example for ENI by filter
          - proto: tcp
            ports:
              - 80
              - 443
            cidr_ip: 0.0.0.0/0
          # Example for VPC Peering by name
          - proto: tcp
            ports:
              - 80
              - 443
            cidr_ip: 0.0.0.0/0
          # Example for VPC Peering by filter
          - proto: tcp
            ports:
              - 80
              - 443
            cidr_ip: 0.0.0.0/0
          # Example for VGW by filter without route propagation
          - proto: tcp
            ports:
              - 80
              - 443
            cidr_ip: 0.0.0.0/0
          # Example for VGW by filter with route propagation
          - proto: tcp
            ports:
              - 80
              - 443
            cidr_ip: 0.0.0.0/0
```

## Testing

#### Requirements

* Docker
* [yamllint](https://github.com/adrienverge/yamllint)

#### Run tests

```bash
# Lint the source files
make lint

# Run integration tests with default Ansible version
make test

# Run integration tests with custom Ansible version
make test ANSIBLE_VERSION=2.4
```
