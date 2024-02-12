---
title: First time terraform (the hard way using libvirt)
date: 2024-02-12 12:06:00 Z
layout: post
---

I decided to terraform to manage VMs on my home lab server.

The first step was to install the Terraform tool (I am using Debian), so I followed their tutorial: [https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli) that worked great, no issues.

Next, I need something simple to prove that something is working, so why not create a simple Hello World project (or copy and paste from somewhere?)

Let's create a main.tf file with contents:

    resource "null_resource" "default" {
      provisioner "local-exec" {
        command = "echo 'Hello World'"
      }
    }

Then, to start this project, you need to run the following:
```
terraform init
```

It downloads if something is needed.

To run this plan, you can run a command to see the terraform plan (what it intends to do):
```
terraform plan
```

And when it looks ok, you can run the command (it will ask you to write yes): 
```
terraform apply
```

```
hello-world$ terraform apply

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  + Create

Terraform will perform the following actions:

  # null_resource.default will be created
  + resource "null_resource" "default" {
      + id = (known after apply)
    }

Plan: 1 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes.

null_resource.default: Creating...
null_resource.default: Provisioning with 'local-exec'...
null_resource.default (local-exec): Executing: ["/bin/sh" "-c" "echo 'Hello World'"]
null_resource.default (local-exec): Hello World
null_resource.default: Creation complete after 0s [id=589244851261473893]
```

Good, at this point, that was simple. But boy, I spent days trying to build a working Debian VM using libvirt.
## First issue: 
I wanted Terraform to manage the remote libvirt server via SSH. All examples didn't work; they complained that the SSH server wasn't in the known host list (I tried adding known hosts and known_hosts, but that just refused to work). So, I went this route:

```
  uri = "qemu+ssh://markom@homelab.lt:9022/system?known_hosts_verify=ignore"
```

## Second issue:
There was a weird permission issue that looked like Terraform couldn't manage VM images; I tried multiple folders and setting permissions to 777, but nothing worked.
Then I found this issue:
https://github.com/dmacvicar/terraform-provider-libvirt/issues/546

So, just changing this file /etc/apparmor.d/libvirt/TEMPLATE.qemu it worked:
```
#
# This profile is for the domain whose UUID matches this file.
#

#include <tunables/global>

profile LIBVIRT_TEMPLATE flags=(attach_disconnected) {
  #include <abstractions/libvirt-qemu>
  /var/lib/libvirt/images/**.qcow2 rwk, /var/lib/libvirt/images/**.raw rwk, /var/lib/libvirt/images/**.img rwk,
}
```

## Third issue:

![Screenshot from 2024-02-12 14-33-00.png](/uploads/Screenshot%20from%202024-02-12%2014-33-00.png)
network_config and user_data can't be set at the same time, so I decided for now go with default network settings (holy cow, I spent too long on this)

## Fourth issue:
For some reason, qemu-guest-agent after installing doesn't start, so I needed to add this line to cloud_init.yml:
```
runcmd:
  - [ systemctl, start, qemu-guest-agent ]
```

## Fifth issue:
![Screenshot from 2024-02-12 14-42-26.png](/uploads/Screenshot%20from%202024-02-12%2014-42-26.png)

The issue is described here: [https://github.com/hashicorp/terraform/issues/3287](https://github.com/hashicorp/terraform/issues/3287)

# Final result:
[https://github.com/Markomas/terraform_debian](https://github.com/Markomas/terraform_debian)
