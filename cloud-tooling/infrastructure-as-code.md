---
description: >-
  Setting up secure SAAS environments on different clouds requires a set of
  basic tools.
---

# Infrastructure as Code

This section explains a SAAS setup that makes it easy for SRE team to manage environments for different clouds.

## Packer

[https://www.packer.io/](https://www.packer.io) is a tool by HashiCorp that is used to create custom base images. These base images typically contain common configs, like a private package repo and config management automation files. Packer stores the custom images in the cloud provider image store, so it can be selected when spinning up new VMs.

## Terraform

[https://www.terraform.io/](https://www.terraform.io) is another tool by HashiCorp. It is an open-source infrastructure as code software tool that provides a consistent CLI workflow to manage hundreds of cloud services. It is used to setup Cloud infrastructure, i.e. VPC, subnets, server, NAT gateways, firewall rules, security groups, any many other things.

Cheat Sheet: [https://acloudguru.com/blog/engineering/the-ultimate-terraform-cheatsheet](https://acloudguru.com/blog/engineering/the-ultimate-terraform-cheatsheet)
