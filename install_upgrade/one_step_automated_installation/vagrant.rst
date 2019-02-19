.. _vagrant_installation:

Run Oro Application Using Vagrant Provision
===========================================

To get familiar with the application functionality or for development purposes, you can install Oro applications with
environment components using `Vagrant <https://www.vagrantup.com/>`_.

Every Oro application has a *Vagrantfile* that enables you to set up a virtual machine with the Oro application via the
`vagrant up` command.

For example, to set up a VM with OroPlatform application v.3.1.2 locally, run:

.. code:: bash

    git clone -b 3.1.2 https://github.com/oroinc/platform-application.git oroapp && cd oroapp
    vagrant up

Once the command has run, you can access the application via the http://localhost:8000/ URL.

The environment in the resulting VM and the provisioning steps in the Vagrantfile replicate the process described in the :ref:`Manual Installation Guide <install-for-dev>` for the relevant application edition.

For more details on preparing the virtual platform for the virtual machine, please see the `Installation and Configuration`_ section below.

.. contents:: Table of Contents
   :local:
   :depth: 2

Installation and Configuration
------------------------------

Requirements
^^^^^^^^^^^^

* Windows, MacOS, Linux, or any other OS supported by Virtualbox and Vagrant
* 2048M available RAM on your PC

Installation Steps
^^^^^^^^^^^^^^^^^^

Start by installing the required software:

1. `Install VirtualBox <https://www.virtualbox.org/wiki/Downloads>`_
2. `Install Vagrant <https://www.vagrantup.com/docs/installation/>`_
3. `Install Git <https://www.atlassian.com/git/tutorials/install-git>`_

When you have installed VirtualBox, Vagrant, and Git, do the following:

1. Get the Oro application source code:

   .. code:: bash

      git clone -b 3.1.2 <oro_application_clone_url> oroapp && cd oroapp

   Replace the <oro_application_clone_url> with the repository URL for the necessary Oro application:

   .. csv-table::
      :widths: 20, 30

      "OroCRM Community Edition","https://github.com/oroinc/crm-application.git"
      "OroCRM Enterprise Edition","https://github.com/oroinc/crm-enterprise-application.git"
      "OroPlatform","https://github.com/oroinc/platform-application.git"

   The *branch* value (in this example *3.1.2*) can be changed to another published `release tag <https://github.com/oroinc/platform-application/releases>`_ of the chosen Oro application with the following constraints:

   .. csv-table::
      :widths: 20, 30

      "OroCRM Community Editions v.3.* and above","The *branch* value must be greather then *3.0.0-rc*"
      "OroCRM Community Editions v.2.6.*","The *branch* value must be greather then *2.6.13*"
      "OroCRM Enterprise Editions v.3.* and above","The *branch* value should be greather then *3.0.0-rc*"
      "OroCRM Enterprise Editions v.2.6.*","The *branch* value should be greather then *2.6.13*"
      "OroPlatform Community Editions v.3.* and above","The *branch* value must be greather then *3.0.0-rc*"
      "OroPlatform Community Editions v.2.6.*","The *branch* value must be greather then *2.6.13*"

   Previously published releases of the Oro Applications do not have Vagrantfiles.

   .. note:: You can download and unpack the archive with Oro application source code instead of using Git repository. For more information, please see :ref:`Get the Oro Application Source Code <installation--get-files>`.

2. Create and configure the virtual machine using Vagrant:

   For Community Editions of the Oro Applications it's enough to run

   .. code:: bash
  
      vagrant up

   For Enterprise Editions of Oro applications, specify the following two environment variables:

   - **gittoken** - `Github token <https://github.com/settings/tokens>`_ use it to install Oro application dependencies (required due to the `Github rate limits <https://getcomposer.org/doc/articles/troubleshooting.md#api-rate-limit-and-oauth-tokens>`_)
   - **licence** - Enterprise Licence key for your Oro Application

   .. code:: bash

      gittoken=39ca9521e1031bfacae976f8d799fa7c54a15edb licence=YourEnterpsiseLicenceKey vagrant up

   Once the command execution is complete and the setup has finished, you can use the Oro application.  Read more on logging in with credentials in the `Usage`_ section.

   .. note:: When you run `vagrant up` for the first time, the Oro application installation may take some time, as the following time-consuming steps happen:

             * Base CentOS/7 box download
             * LEMP stack installation on the guest system
             * Installation of composer dependencies for Oro application
             * Oro application installation; note that demo data loading takes extra time.

      The total time for the environment to get up and running depends on multiple factors, such as the internet connection speed, CPU frequency, etc. It usually takes from 2 to 15 minutes.

Customize Installation Process
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To customize the default installation settings, modify the `Provision configuration` section of the Vagrantfile (refer to the commands and inline comments for more information).

The default values for the most important settings are:

**Database settings**

.. code:: bash

   DB_USER=dbuser
   DB_PASSWORD=DBP@ssword123
   DB_NAME=oro

**Application settings**

.. code:: bash

   APP_HOST=localhost
   APP_USER=admin
   APP_PASSWORD=adminpass
   APP_LOAD_DEMO_DATA=y    # y | n (whether to perform loading demo data during installation)

To customize the application hostname:

* Set the new APP_HOST parameter value in the Vagrantfile (e.g. `yourdomain.local`)
* Map the new hostname to the application host IP address in your local `hosts <https://en.wikipedia.org/wiki/Hosts_(file)>`_ file, like in the following examples:

  .. code:: bash

     192.168.33.10 yourdomain.local www.yourdomain.local

Now you can open the Oro application in a browser via the http://yourdomain.local/ URL.

Running Multiple Virtual Machines
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To run multiple virtual machines simultaneously on a single host, ensure that every virtual instance uses a unique forwarded port. Before running an additional instance, modify its forwarded port in the *host* section of the *config.vm.network "forwarded_port"* setting in the Vagrant file.  You can increment the value for every new virtual instance, e.g. **instance A** can have *config.vm.network "forwarded_port", guest: 80, host: 8000* configuration, and **instance B** can have *config.vm.network "forwarded_port", guest: 80, host: 8001*.

Usage
-----

Access the Installed Oro Application
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Once the VM setup has finished, you can access the application in your browser with the credentials defined by your installation configuration.

The default login details are:

* *Application URL*: http://localhost:8000/
* *Admin Login*: admin
* *Admin Password*: adminpass

If you have changed the application host, admin login, or password, please refer to the Vagrantfile for these details.

Shared Working Directory
^^^^^^^^^^^^^^^^^^^^^^^^

Vagrant maps the working directory on your host machine to the */vagrant* directory in the virtual machine file system.

Once the VM is up, any changes to the files in the host working directory are applied to the */vagrant* directory in the virtual machine file system, and vice versa.

.. note::

   For installation, the application source code is copied from */vagrant* folder to the application root folder (*/usr/share/nginx/html/oroapp/*) in the VM file system.

SSH Access to the Virtual Machine
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To connect to the virtual machine via SSH, run the following command in the working directory on your host machine:

.. code:: bash

   vagrant ssh

You will be logged in the virtual machine as *vagrant* user with *sudo* permission (you do not need a password to use the *sudo* command).

To configure SSH access to the virtual machine for the utilities that run on the host machine, like IDE, get the explicit SSH credentials by running the following command:

.. code:: bash

   vagrant ssh-config

Access Oro Application Database
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

In the virtual machine, you can access the DB server using credentials provided in the *Provision configuration* section of
the Vagrantfile. The default credentials are *dbuser:DBP@ssword123*.

To access the database from the host machine, configure and use the SSH connection to the guest machine database server, as described in the `SSH Access to the Virtual Machine`_ section above.

Vagrant Сommands
^^^^^^^^^^^^^^^^

* `vagrant up` -- Creates and configures the virtual machine according to the vagrantfile. Unless `vagrant destroy` has been launched on the consecutive runs `vagrant up` powers on the virtual machine. The provisioning script defined in `config.vm.provision` variable in the Vagrantfile is run only once.

* `vagrant halt` -- Stops the virtual machine and saves the virtual machine image (without the current RAM state) to the host hard drive.

* `vagrant suspend` -- Stops the virtual machine and saves the virtual machine image and the current RAM state the host hard drive.

* `vagrant destroy` -- Destroys the VM and frees the resources of the host machine.

For more information, please see the `official Vagrant documentation <https://www.vagrantup.com/docs/>`_.
