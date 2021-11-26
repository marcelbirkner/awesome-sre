---
description: Ansible is configuration management tool written in python.
---

# Ansible

Ansible is a radically simple IT automation system. It handles configuration management, application deployment, cloud provisioning, ad-hoc task execution, network automation, and multi-node orchestration. Ansible makes complex changes like zero-downtime rolling updates with load balancers easy. More information on the Ansible [website](https://ansible.com).

## Configure Ansible for AWS

Create an EC2 role with required permissions so Ansible can read EC2 system information via the AWS API. Attach this role to the EC2 instance where Ansible is installed.

![](<../.gitbook/assets/image (1).png>)

Install Ansible on EC2 instance an prepare configuration files&#x20;

**ansible.cfg**

```
[inventory]
enable_plugins = auto

[defaults]
inventory = eu-west-1.aws_ec2.yaml
deprecation_warnings = False
```

**aws\_ec2.yaml**

```
---

plugin: aws_ec2

regions:
  - eu-west-1

filters:
  instance-state-name : running

hostnames:
  - tag:Name
```

**eu-west-1.aws\_ec2.yaml**

```
---
plugin: aws_ec2
regions:
  - eu-west-1
filters:
  instance-state-name : running
hostnames:
  - tag:Name
```

## **Run ansible commands**

Display the configured inventory as Ansible sees it.

```
ansible-inventory --graph
```

```

@all:
  |--@aws_ec2:
  |  |--server-1-eu-west-1.instana.io
  |  |--server-2-eu-west-1.instana.io
  |  |--server-3-eu-west-1.instana.io
  |  |--server-4-eu-west-1.instana.io
 ...
```

Get Kernel information from all hosts

```
ansible aws_ec2 --become -a 'uname -r'
```

```
server-1-eu-west-1.instana.io | CHANGED | rc=0 >>
4.15.0-1045-aws
server-2-eu-west-1.instana.io | CHANGED | rc=0 >>
5.4.0-1055-aws
...
```
