---
author:
  name: Alex Fornuto
  email: docs@linode.com
description: 'How to upgrade from Debian 7 (Wheezy) to Debian 8 (Jessie).'
keywords: 'debian,upgrade,wheezy,jessie'
license: '[CC BY-ND 3.0](http://creativecommons.org/licenses/by-nd/3.0/us/)'
modified: Wednesday, April 29th, 2015
modified_by:
  name: Alex Fornuto
published: ''
title: 'Upgrading to Debian 8 (Jessie)'
---

Debian 8 (Jessie) was released in April 2015. This guide explains how to upgrade your system from Debian 7 (Wheezy) to Debian 8. Before you begin, make sure that you have a working [backup](/docs/platform/backup-service) or a copy of your data.

{: .note }
> If you use the Apache web server, be aware that Debian 8 moves from Apache 2.2 to 2.4. This version change requires several adjustments to configuration files, and can break an existing website. Please follow our [Upgrading Apache](https://www.linode.com/docs/security/upgrading/updating-virtual-host-settings-from-apache-2-2-to-apache-2-4) guide before continuing.

## Preparing to Upgrade

Prepare your Linode for Debian 8 by installing updates, backing up your Linode, checking your kernel, stopping services, and starting a screen session.

### Installing Available Updates

You should install all available updates for Debian 7 before upgrading to Debian 8.

    sudo apt-get update && sudo apt-get upgrade -y


### Backing Up Your Linode

It's a good idea to [back up](/docs/platform/backup-service) your Linode before performing a major upgrade. If you subscribe to the Linode Backup Service, we recommend that you [take a manual snapshot](/docs/platform/backup-service/#taking-a-manual-snapshot) before upgrading to Debian 8. If you use another backup service or application, we recommend that you make a manual backup now. You may also want to back up your configuration files (usually located in /etc/) in case they have changed in later versions of the software you are using.

### Checking Your Kernel

Verify that your Linode is using the latest supported kernel. See [Applying Kernel Updates](/docs/uptime/monitoring-and-maintaining-your-server/#applying-kernel-updates) for more information.

### Stopping Services

We recommend that you stop as many services as possible before upgrading to Debian 8. This includes web server daemons (Apache and nginx), database servers (PostgreSQL and MySQL), and any other non-critical services. To stop a service, enter the following command, replacing `apache2` with the name of the service you want to stop:

    sudo service apache2 stop

### Starting a Screen Session

We recommend that you start a screen session to ensure that the updates will continue to install in the unlikely event you are disconnected from the Linode during the upgrade process. Here's how to install `screen` and start a screen session:

1.  Install screen by entering the following command:

        sudo apt-get install screen

2.  After installation has completed, start a screen session by entering the following command:

        screen

3.  If you are disconnected from your server, you can reconnect to the screen session by entering the following command:

        screen -rd

You are now ready to install Debian 8 on your Linode.

Upgrading to Debian 8
---------------------

Here's how to upgrade from Debian 7 to Debian 8:

1.  Edit your `/etc/apt/sources.list` file and change all instances of `wheezy` to `jessie`. Once you have finished, your `/etc/apt/sources.list` should resemble the following:

    {: .file-excerpt }
    /etc/apt/sources.list
    :   ~~~
        deb http://ftp.us.debian.org/debian/ wheezy main
        deb-src http://ftp.us.debian.org/debian/ wheezy main

        deb http://security.debian.org/ wheezy/updates main
        deb-src http://security.debian.org/ wheezy/updates main

        # wheezy-updates, previously known as 'volatile'
        deb http://ftp.us.debian.org/debian/ wheezy-updates main
        deb-src http://ftp.us.debian.org/debian/ wheezy-updates main
        ~~~

    {: .note}
    >
    > Check your /etc/apt/sources.list.d for additional package repositories, and ensure that they are querying for packages from `jessie`.  You will need to check with the maintainers of each package to ensure that their own repositories have been updated.

2.  Enter the following command to update your package lists:

        sudo apt-get update

3.  Enter the following command to grab the latest versions of key system utilities:

        sudo apt-get install apt dpkg aptitude

4.  After the package updates have completed, upgrade your system by entering the following command. The upgrade will download and install numerous packages. This step may take a while to complete.

        sudo apt-get dist-upgrade

     {: .note }
    >
    > Services using "NSS" (Network Security Services) and "PAM" (Pluggable Authentication Modules) will need to be restarted. In most cases the default list of services to be restarted is fine. If you have additional services that you run that use NSS or PAM, please add them to the list.

    During the upgrade process, configuration files that you've modified and require updates will be presented for manual review. Here's an example:

        Configuration file '/etc/mysql/my.cnf'
        ==> Modified (by you or by a script) since installation.
        ==> Package distributor has shipped an updated version.
        What would you like to do about it ?  Your options are:
        Y or I  : install the package maintainer's version
        N or O  : keep your currently-installed version
        D     : show the differences between the versions


5.  Once the system is updated, reboot your system using the [Linode Manager](https://manager.linode.com) to make sure that there were no problems during the upgrade. While your system reboots, you can watch your Linode's console for errors using the AJAX terminal or [Lish](/docs/troubleshooting/using-lish-the-linode-shell).

<br>
Your Linode is now running Debian 8!

Troubleshooting Problems
------------------------

Errors about packages that are removed and not purged may be resolved by installing the latest version of the software or purging the old package. For example, if you receive an error about MySQL, you could try to resolve the issue by entering the following command:

    apt-get install mysql-server

If the above step does not work for the package you are trying to install, you may purge the package with the following command after **ensuring that you have a backup** of the program's data and configuration:

    apt-get remove --purge mysql-server


