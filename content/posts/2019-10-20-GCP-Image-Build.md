---
title: Building Images for GCP
date: 2019-09-11T04:00:00Z
author: "Cliff Hults"
categories: [DevOps]
tags: [devops, packer, terraform, ansible]
---

In my last post, I opened with the fact that my company has decided to dive into the world of GCP to get ahead of most of the market in our space. With a few of us being tasked for this initive, I decided to take it upon myself to look into [Packer](https://packer.io). The goal would be to roll our homebrewed software/OS image into an automated build process to make images in GCP (or AWS, vSphere, etc.).

Our current image is based on CentOS 6.8, so I needed to create kickstart scripts, some provisioning scripts, and then create some post processing steps to get it all into GCP without interaction.

# TODO
# GRUB changes
# Google Guest environment challenges.
