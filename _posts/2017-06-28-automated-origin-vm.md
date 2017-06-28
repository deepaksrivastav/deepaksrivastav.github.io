---
layout: post
title: "Automated OpenShift Origin all-in-one VM creation"
date: 2017-06-28 13:52
categories: openshift containers ansible
---

I used Vagrant and Ansible to automate the creation of an OpenShift Origin based all-in-one VM for development purposes.

NOTE: Tested only on Fedora 25

### Step 1: Install Pre-requisites
If you are on Fedora (like me):
``` bash
$ dnf install @virtualization vagrant ansible
```

### Step 2: Create and provision the vagrant box
``` bash
$ git clone https://github.com/deepaksrivastav/openshift-origin-allinone.git
$ cd openshift-origin-allinone
$ vagrant up
```

Grab a cup of coffe!

If the installation was successful, you should see something like this :

``` bash
TASK [Installation Details] ****************************************************
ok: [default] => {
    "msg": "Login into openshift console at https://192.168.121.160:8443 with username/password as admin/admin"
}

PLAY RECAP *********************************************************************
default                    : ok=4    changed=2    unreachable=0    failed=0  
```

PS: Do not re-provision the appliance if installation fails. The playbooks are not idempotent (yet)
