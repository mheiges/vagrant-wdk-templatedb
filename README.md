# WDK-Strategies TemplateDB

A virtualized server hosting TemplateDB. This is currently an unsupported project used 
as a test bed for EuPathDB BRC infrastructure reorganization and deployment code
rewrites (primarily via Puppet).

### Install Dependencies

* Download and install [Vagrant](https://www.vagrantup.com/downloads.html)
* Download and install [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
* Download and install [Ansible](http://docs.ansible.com/ansible/intro_installation.html)

## Clone project
* Clone the project `git clone git@github.com:mheiges/vagrant-wdk-templatedb.git`

## Configure

Copy `config/private.yml.sample` to `config/private.yml` and supply desired values. 
`private.yml` is excluded from git commits to reduce the changes of publishing
sensitive data. The other file in the `config` directory, `config/common.yml`,
typically does not need to be changed.

For the brave, all the configuration features of Vagrant and VirtualBox are of course
available as well, but there is no requirement that you tinker with them.

## VPN setup, if needed

The website uses a UGA Postgres database. If off-campus you will need a VPN setup in 
advance so the WDK can connect. If the WDK can not connect to the database it
will not start. There are many options for a VPN. 
[sshuttle](https://github.com/mheiges/sshuttle) is one of them.

    sshuttle -r <user>@<ugahost> <uga ip subnet>.0/24

`<ugahost>` is a server that has clear access to the Postgres database.
Tunneling is limited to packets destined to `<uga ip subnet>` .

## Start the virtual machine

In the project dir run `vagrant up`. Vagrant will download the base box on the first 
run and cache it for future deployments. The base box represents a server freshly
provisioned by Puppet. Here, Vagrant leverages Ansible to deploy and configure
a TemplateDB site, simulating how project staff would interact with the server. This
provides the opportunity to validate our Puppet deployments.

## Accessing the website

Vagrant forwards the tomcat port to 9380 on your host computer, so the 
website is reachable from your host at

[http://localhost:9380/strategies/](http://localhost:9380/strategies/)

## Interacting with the server

See Vagrant and Ansible documentation for details.

#### Log in

    vagrant ssh

You will log in as the `vagrant` user.

#### Shutdown and preserve state

    vagrant halt

#### Shutdown and remove the instance (losing any changes you've made on the guest)

    vagrant destroy
    
#### Run the Vagrantfile provisioning steps

    vagrant provision

#### Run ansible

Running `vagrant provision` will also run ansible but if you can also run ansible
manually (useful if you want to use `--skip` options). The actual ansible command
uses several long options so a convenience script is included to simplify it.

    bin/runansible

#### TemplateDB stuff

The source code and build artifacts are under `/home/vagrant/TemplateDB`, modeling
the directory structure of EuPathDB BRC servers.

The `instance_manager` utility is used to manage Tomcat and deployed webapps.
  

#### Doing stuff as root

The vagrant user has full sudo permissions, no password required.

    sudo <command>
