---
layout: post
title:  "Tips Using Terraform in Production"
date:   2017-01-16 16:17:31 -0800
categories: terraform aws devops
---

## Recommended project structure

At first we tried to separate stage and prod envs by having a structure like so:
```
├── service.tf
├── prod
│   └── terraform.tfvars
│   └── .terraform
└── stage
   └── terraform.tfvars
    └── .terraform
```

This allowed us to share the same terraform file for both staging and production, and besides a minor annoyance with having to manually specify the variable file path when using terraform, or pushing to a stage or prod Atlas environment, it worked smoothly. Notice that we kept separate state files per environment which is a really good thing to do.

However, We eventually made an effort to start DRYing up different service's Terraform files by using shared Terraform [Modules](https://www.terraform.io/docs/modules/index.html).
Once we started doing this, we quickly began to realize that each service's tf file was mostly just passing variables to modules, which contained the meat of the actual setup and config for each resource.

Since modules brought down the cost and complexity of maintaining separate tf files for different environments, the infra for our services started to look like this:
```
├── prod
│   ├── service.tf
│   └── terraform.tfvars
│   └── .terraform
└── stage
   ├── service.tf
   └── terraform.tfvars
   └── .terraform
```
This is similar to Hashicorp's own [best practices](https://github.com/hashicorp/best-practices/tree/master/terraform/providers/aws). I also recommend a `global` Terraform file for managing things shared between all environments (Which depends on your usage but could be: Route53 Zones/Records, VPC subnets, etc).

## Tag your resources liberally

It is very useful to see inside the aws console or via the api that something is managed in Terraform.
I always put at least a `"terraform" = "true"` tag on all of the resources in my terraform modules. An `env` flag is also very useful.

## Terraforming

When we started using Terraform we already had resources defined in AWS. We used [terraforming](https://github.com/dtan4/terraforming) to export hundreds of DNS records in seconds, which saved a lot of busy work making the tf files from hand. Somethings are still not implemented in terraforming, but a lot of the aws services have terraforming implementations.

## Write tests

'Infrastructure as code' is a great idea, but if it's really code we should test it as code right?

I think the best current way to test terraform is to use [kitchen-terraform](https://github.com/newcontext-oss/kitchen-terraform/blob/master/examples/aws_provider/getting_started.md). The previous link shows an example of how to use it with aws terraform resources.

## Conditionals!

Terraform 0.8 added [conditionals](https://www.terraform.io/docs/configuration/interpolation.html#conditionals) Which was desperately needed.
Conditional is a ternary operator that can be used in HCL. Before, if you wanted to do something conditionally you had to use a variable and `count` which was pretty awkward.
