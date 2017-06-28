---
layout: post
title: "OpenShift Origin Installation"
date: 2017-06-28 09:36
categories: openshift containers
---

This is a quicker way of installing an all-in-one OpenShift Appliance on Centos 7 for development purposes. I did all this on my Fedora 25 machine with libvirt installed.
This will not only install the latest available stable version of OpenShift Origin, but also deploy Docker registry, Router and confgure NFS on the appliance. It will also configure persistent store for docker registry.

**Step 1** Start a centos vagrant box

``` bash
$ vagrant init centos/7
```

**Step 2** Upgrade RAM and CPU

``` ruby

Vagrant.configure("2") do |config|
  config.vm.box = "centos/7"
  config.vm.provider :libvirt do |domain|
    domain.memory = 4068
    domain.cpus = 2
  end
end

```

**Step 3** Start the vagrant box and check the IP address of the vagrant box

``` bash
$ vagrant up
$ vagrant ssh-config

Host default
  HostName 192.168.121.14
  User vagrant
  Port 22
  UserKnownHostsFile /dev/null
  StrictHostKeyChecking no
  PasswordAuthentication no
  IdentityFile /home/deepak/personal/openshift/allinone/.vagrant/machines/default/libvirt/private_key
  IdentitiesOnly yes
  LogLevel FATAL

```

**Step 4** SSH into the Vagrant box

``` bash
$ vagrant ssh
```

**Step 5** Update the OS and install some pre-requisites.
``` bash
$ sudo yum -y update
$ sudo yum -y install -y git vim
```

**Step 6** Add centos-release-openshift-origin repository and install atomic-openshift-utils
``` bash
$ sudo yum install -y centos-release-openshift-origin
$ sudo yum install -y atomic-openshift-utils
```

**Step 7** Generate SSH Keys and add it to authorized keys
``` bash
$ ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/home/vagrant/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/vagrant/.ssh/id_rsa.
Your public key has been saved in /home/vagrant/.ssh/id_rsa.pub.
The key fingerprint is:
4d:93:2e:bd:3e:36:4a:e7:e3:71:e1:ed:63:8a:9a:dd vagrant@localhost.localdomain
The key's randomart image is:
+--[ RSA 2048]----+
|                 |
|           .     |
|          +      |
|         = .     |
|        S + .    |
|         . o o   |
|        . + o .  |
|       . *== .o  |
|        +=*+Eo.. |
+-----------------+

$ cat .ssh/id_rsa.pub >> .ssh/authorized_keys
```

**Step 8** Ansible inventory file at /etc/ansible/hosts
``` ini
[OSEv3:children]
masters
nodes
etcd
nfs

[OSEv3:vars]
# ansible options
ansible_user=vagrant
ansible_become=True

# openshift options
deployment_type=origin
openshift_master_default_subdomain=IP_OF_VAGRANT_BOX.xip.io
openshift_master_identity_providers=[{'name': 'htpasswd_auth', 'login': 'true', 'challenge': 'true', 'kind': 'HTPasswdPasswordIdentityProvider', 'filename': '/etc/origin/master/htpasswd'}]

[masters]
IP_OF_VAGRANT_BOX

[etcd]
IP_OF_VAGRANT_BOX

[nfs]
IP_OF_VAGRANT_BOX

[nodes]
IP_OF_VAGRANT_BOX openshift_node_labels="{'region': 'infra', 'zone': 'default'}" openshift_schedulable=True

```

**Step 9** Check connectivity with ansible. You will see the Host authenticity prompt only for the first time.
``` bash
$ ansible -i inventory all -m ping
The authenticity of host '192.168.121.14 (192.168.121.14)' can't be established.
ECDSA key fingerprint is 74:7e:ee:09:37:8f:78:1a:7d:4e:56:58:e8:fc:3d:aa.
Are you sure you want to continue connecting (yes/no)? yes

192.168.121.14 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}

```

**Step 10** Install OpenShift.
``` bash
$ ansible-playbook /usr/share/ansible/openshift-ansible/playbooks/byo/config.yml
```

This will take a while to complete and the last few lines should looks like this
```
PLAY RECAP *********************************************************************
192.168.121.14             : ok=575  changed=165  unreachable=0    failed=0   
localhost                  : ok=15   changed=0    unreachable=0    failed=0   
```

**Step 11** Check version of OCP installed and if router and registry were deployed
``` bash
# check version of openshift installed
$  oc version
oc v1.4.1
kubernetes v1.4.0+776c994
features: Basic-Auth GSSAPI Kerberos SPNEGO

Server https://192.168.121.14:8443
openshift v1.4.1
kubernetes v1.4.0+776c994

# Check if registry, registry-console and router were deployed
$ oc get pods -n default
NAME                       READY     STATUS    RESTARTS   AGE
docker-registry-2-tjm5m    1/1       Running   0          3m
registry-console-1-pm95f   1/1       Running   0          3m
router-1-ufwot             1/1       Running   0          3m
```

**Step 12** Check if NFS shares were created and that registry uses it
``` bash
# Check location of NFS Shares
$ showmount -e
Export list for localhost.localdomain:
/exports/logging-es *
/exports/metrics    *
/exports/registry   *

# Check if registry is configured as persistent and it uses the NFS share
$ oc get pv
NAME              CAPACITY   ACCESSMODES   RECLAIMPOLICY   STATUS    CLAIM                    REASON    AGE
registry-volume   5Gi        RWX           Retain          Bound     default/registry-claim             6m
```

**Step 13** Create htpasswd file, add some users and assign roles (htpasswd -bc /etc/origin/master/htpasswd username password)
``` bash
$ sudo htpasswd -bc /etc/origin/master/htpasswd admin admin
$ oadm policy add-cluster-role-to-user cluster-admin admin
```

**Step 14** Login into openshift console at https://IP_OF_VAGRANT_BOX:8443
