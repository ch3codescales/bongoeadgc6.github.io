---
date: 2019-09-11T04:00:00Z
author: "Cliff Hults"
title: Vault Journey
description: "Using Terraform and HashiCorp Vault to templatize configs and manage rotating secrets for reproducible Compute Engine VMs on GCP."
categories: [DevOps]
tags: [vault, devops, terraform, ansible]
---

Being a good SysAdmin requires some sense of laziness. In the spirit of that approach, I've spent some time looking into Hashicorp's Terraform and Red Hat's Ansible tools in my organization's route to Google Cloud Platform. I wanted a method to create reproducible Compute Engine VMs that would allow us to easily create multiple hosts with minor changes quickly and easily. 

I decided that with the use of Terraform, Vault (also from Hashicorp) would allow me to templatize the configs, as well as, create secrets/passwords that would be randomized, able to be called at will, and even automatically rolled with a configured frequency.
