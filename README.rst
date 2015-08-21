==========
 Rippowam
==========

Rippowam is an ansible playbook for setting up flavors of OpenStack on top of
RPM based Operating systems.  The current focus is on the RHEL-OSP7 release
and RHEL 7.2 Base Operating system.

Rippowam has a sister project, called Ossipee_ that creates the network and 
virtual machines necessary for installing and demonstrating Rippowam.

.. _Ossipee: https://github.com/admiyo/ossipee/

Ossipee create an inventory file used to populate the initial variables and
host entries to run Rippowam.  An Example of the inventory file is at the
bottom of this document.  Ossipee use e $USER as the default name for
the deployment, and many things make use of the name, such as the
hostname and Kerberos Realm. You will see the strings yourname and
YOURNAME in this document that are generated from the name.

Running
=======

To run Rippowam:

  ansible-playbook -i ~/.ossipee/inventory/yourname.ini ~/devel/rippowam/site.yml 


Once the playbook completes, you should have a working IPA server and
OpenStack deployment.

Hostnames
=========

It is easiest to work with the machines via hostnames.  Add entries to
/.etc/hosts for the publically accessable IP addresses of the two
hosts such as:

  10.16.19.101 ipa.yourname.test
  10.16.18.245 openstack.yourname.test

You should have ssh access to the hosts using an SSH keypair.  

Kerberos
========

To enable Kerberos, scp the krb5.conf file from the ipa server:

  scp ipa.yourname.test:/etc/krb5.conf /home/yourname/.ossipee/inventory/yourname.krb5.conf
  export KRB5_CONFIG=/home/yourname/.ossipee/inventory/yourname.krb5.conf
  kinit admin@YOURNAME.TEST

The password comes from the inventory file.

You should be able to ssh to the ipa server with

  ssh -K ipa.yourname.test

To test the ipa web UI browse to

  https://ipa.yourname.test




Sample inventory file
=====================

  [openstack]
  10.16.19.101

  [openstack:vars]
  ipa_server_password=FreeIPA4All
  ipa_domain=yourname.test
  ipa_realm=YOURNAME.TEST
  cloud_user=cloud-user
  ipa_admin_user_password=FreeIPA4All
  ipa_forwarder=192.168.52.3
  nameserver=192.168.52.4

  [ipa]
  10.16.18.245

  [ipa:vars]
  ipa_server_password=FreeIPA4All
  ipa_domain=yourname.test
  ipa_realm=YOURNAME.TEST
  cloud_user=cloud-user
  ipa_admin_user_password=FreeIPA4All
  ipa_forwarder=192.168.52.3
  nameserver=192.168.52.4

  [ipa_clients]
  10.16.19.101
  [%ipa_clients:vars]
  ipa_server_password=FreeIPA4All
  ipa_domain=yourname.test
  ipa_realm=YOURNAME.TEST
  cloud_user=cloud-user
  ipa_admin_user_password=FreeIPA4All
  ipa_forwarder=192.168.52.3
