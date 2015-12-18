# Vagrant CakePHP 

Vagrant CakePHP creates a Vagrant installation for CakePHP using Chef with the following features:

- Ubuntu 14.04 LTS Trusty Tahr
- Nginx 1.6
- PHP 5.5
- Ruby 2.1
- Percona MySQL 5.6
- Postgres 9.3
- Redis 2.8
- Memcached 1.4
- Git 2.2
- Composer
- The ruby gems `heroku`, `hub`, `travis` and `travis-lint`

## Requirements

- [VirtualBox](https://www.virtualbox.org/wiki/Downloads). Tested on 4.3.x.
- [Vagrant](http://www.vagrantup.com/downloads.html). Tested on 1.6+

## Installation

Download and install both VirtualBox and Vagrant for your particular operating system.

Once those are downloaded, open up a terminal. We'll need to clone this repository and setup vagrant:

```bash
git clone https://github.com/szerhoudi/vagrant-cakephp.git
cd vagrant-cakephp
```

#### Custom App Name

At the end of the install you will be accessing your CakePHP app using either `http://192.168.13.37/` or a custom domain name: `app.dev`.
To customize your domain name, please edit the following files:

    Vagrantfile - line 60 : config.vm.hostname = "your-custom-app.dev",
	cookbooks/nginx/attributes/default.rb - line 18 : default['nginx']['server_name'] = 'your-custom-app'


Now we need to setup the vagrant installation:

```bash
vagrant plugin install vagrant-vbguest
vagrant up
```

`vagrant-vbguest` is a Vagrant plugin which automatically installs the host's VirtualBox Guest Additions on the guest system.

It may take a bit to download the Vagrant box, but once that is done, you will be prompted for your laptop password. This is so we can properly expose the IP of the vagrant instance to your machine. Type in your password and let it continue running.

Once it is done, browse to `http://192.168.13.37/` in your browser, and you should have some sort of `It works!` page! At this point you can set your virtualhosts to point at the instance for maximum win.

#### Custom Domain Name

If you want to access the site using a custom domain name, edit your `/etc/hosts` file to have the following line:

    192.168.13.37 www.app.dev app.dev or 192.168.13.37 www.your-custom-app.dev your-custom-app.dev if you change the app name as shown above


#### Database Access

MySQL is available at `192.168.13.37:3306` with either of the following credentials:

- `root:root`
- `user:password`

For an SSH connection (`SSH user:vagrant`), use the Private Key found in :

- `vagrant-cakephp/.vagrant/machines/default/virtualbox/private_key`

Postgres is available at `192.168.13.37:5432` with either of the following credentials:

- `postgres:password`
- `username:password`


## Starting/Stopping Work

You normally wont want to have the instance running full time. To pause it, simply perform the following in the command line:

```bash
vagrant suspend
```

You will no longer be able to access the instance after doing this. To continue working, issue the following command:

```bash
vagrant resume
```

You can also use `vagrant halt` and `vagrant up` for shutting down and booting the virtual machine.

## Updating Vagrant

Running `vagrant provision` will reprovision the instance. You won't normally need to do the things in the **Installation** section, but this will ensure your setup is as up-to-date as possible.

If there are any updates to the vagrant setup, such as a new feature, new site hosted within, or new service, simply do the following in a terminal:

```bash
git pull origin master
vagrant reload --provision
```

## Destroying the virtual machine

Simply execute this command within the folder where `Vagrantfile` is located:

```bash
vagrant destroy
```

This will destroy your vagrant installation, and you can proceed to remove the project folder from your computer.

