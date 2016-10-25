Manage FreeBSD on EdgeRouter Lite with Ansible
==============================================

Introduction
------------

This repo is a quick start way to begin configuration management with Ansible
of FreeBSD on Ubiquiti EdgeRouter Lite (ERL), assuming these:

* Ansible is installed on your local machine.
* On ERL:

  * You used `freebsd-ERL-build <https://github.com/cperciva/freebsd-ERL-build>`_ to build OS image installed on ERL.
  * *root* user has no password set.
  * A user called *ubnt* exists.
  * Network and remote access through SSH on eth2 (octe2) is working.

* Network

  * IPv4 and IPv6 are functioning and desired at the edge of the network (cable modem in my case).
  * You want to override the MAC address of eth0 (octe0) to something else. I wanted to do it so I could swap in multiple gateway devices without needing to contact my service provider.
  * You want to use the default network config from freebsd-ERL-build.

SSH Config
----------

Configure SSH client config on your local machine. Add this to
*~/.ssh/config*. Modify as needed.

::

    Host erl-admin
        HostName 192.168.2.1
        Port 22
        User ubnt
        AddressFamily inet
        CheckHostIP no
        StrictHostKeyChecking no
        UserKnownHostsFile /dev/null

Since I rebuild ERL many times I use **highly insecure** SSH
settings. You may want to use more secure settings for *CheckHostIP*,
*StrictHostKeyChecking*, and *UserKnownHostsFile*, especially when using
production boxes.

Copy SSH key to remote machine.

::

    $ ssh-copy-id -i ~/.ssh/id_rsa erl-admin

Ansible Config
--------------

Review my settings in *ansible.cfg* and *hosts* files in this repo. Edit them
as desired.

Bootstrap
---------

pkgng is already bootstrapped during OS image build. Installing Python 2.7 is
required before the playbook can run. This is done outside of a playbook
because I like it that way. I have not tried running this in a playbook.

Install Python2.7.

::

    $ ansible --module-name raw --args "/usr/sbin/pkg install -y python27" --become --become-method=su erl-admin


Playbook
--------

Look at the example playbook and modify vars as needed before running it.

::

    $ ansible-playbook play-example.yml

If your assumptions are different from mine then read all the templates in
various roles and modify them as desired.
