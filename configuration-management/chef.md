---
description: >-
  Chef is a configuration management technology used to automate the
  infrastructure provisioning. It is developed on the basis of Ruby DSL language
---

# Chef

Chef is a configuration management **technology used to automate the infrastructure provisioning**. It is developed on the basis of Ruby DSL language. It is used to streamline the task of configuration and managing the company's server. It has the capability to get integrated with any of the cloud technology.

## chef-cascade

Chef Cascade is a minimally functional chef client, that downloads its chef artifacts via OS packages. It can be used standalone or with the rest of the cascade integration for a blown distributed chef solution without the need for a chef server.

```
chef-cascade

# dry run
chef-cascade -W

# update single recipe only
chef-cascade -o "recipe[cookbook::recipe]"
```
