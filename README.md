# Ansible role: AWS EC2 Security Group

This role handles the creation of AWS EC2 Security Group's

[![Build Status](https://travis-ci.org/Flaconi/ansible-role-aws-ec2-security-group.svg?branch=master)](https://travis-ci.org/Flaconi/ansible-role-aws-ec2-security-group)
[![Version](https://img.shields.io/github/tag/Flaconi/ansible-role-aws-ec2-security-group.svg)](https://github.com/Flaconi/ansible-role-aws-ec2-security-group/tags)

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
        #Rules reference https://docs.ansible.com/ansible/latest/modules/ec2_group_module.html#parameter-rules
        rules:
          - proto: tcp
            ports:
              - 80
              - 443
            group_name: devops-test-sg
            cidr_ip: 0.0.0.0/0
            rule_desc: test rule
            group_id:
              - sg-edcd9784
              - sg-edcd9785

  # Create Security Groups by VPC filter
  - vpc_filter:
      - key: "tag:Name"
        val: "devops-test-vpc"
      - key: "tag:env"
        val: playground
      - key: "tag:department"
        val: devops
    security_groups:
      - name: devops-test-sg-2
        #Rules reference https://docs.ansible.com/ansible/latest/modules/ec2_group_module.html#parameter-rules
        rules:
          # Example for specifying single port
          - proto: tcp
            ports: 22
            cidr_ip: 0.0.0.0/0
          # Example for specifying single multiple ports
          - proto: tcp
            ports:
              - 80
              - 443
              - 3306
            cidr_ip: 0.0.0.0/0
          # Example for specifying port range
          - proto: tcp
            ports:
              - 8080-8099
            group_name: devops-test-sg-2
            cidr_ip: 0.0.0.0/0
            rule_desc: test rule
            group_id:
              - sg-edcd9784
              - sg-edcd9785
```

#### All available parameter
```yml

aws_ec2_security_group_vpc_filter_additional:
  - key: state
    val: available

aws_ec2_security_groups:

  # Create Security Group for by VPC name
  - vpc_name: devops-test-vpc
    region: eu-central-1
    security_groups:
      - name: devops-test-sg
        #Rules reference https://docs.ansible.com/ansible/latest/modules/ec2_group_module.html#parameter-rules
        rules:
          - proto: tcp
            ports:
              - 80
              - 443
            group_name: devops-test-sg
            cidr_ip: 0.0.0.0/0
            rule_desc: test rule
            group_id:
              - sg-edcd9784
              - sg-edcd9785

  # Create Security Groups by VPC filter
  - vpc_filter:
      - key: "tag:Name"
        val: "devops-test-vpc"
      - key: "tag:env"
        val: playground
      - key: "tag:department"
        val: devops
    region: eu-central-1
    security_groups:
      - name: devops-test-sg-2
        #Rules reference https://docs.ansible.com/ansible/latest/modules/ec2_group_module.html#parameter-rules
        rules:
          # Example for specifying single port
          - proto: tcp
            ports: 22
            cidr_ip: 0.0.0.0/0
          # Example for specifying single multiple ports
          - proto: tcp
            ports:
              - 80
              - 443
              - 3306
            cidr_ip: 0.0.0.0/0
          # Example for specifying port range
          - proto: tcp
            ports:
              - 8080-8099
            group_name: devops-test-sg-2
            cidr_ip: 0.0.0.0/0
            rule_desc: test rule
            group_id:
              - sg-edcd9784
              - sg-edcd9785
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
make test ANSIBLE_VERSION=2.5
```
