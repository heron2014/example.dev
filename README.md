#Example.dev

## This is a fully detailed tutorial for project that uses [Bedrock](https://roots.io/bedrock/) Wordpress development framework, [Trellis](https://github.com/roots/trellis) and [Sage](https://github.com/roots/sage)

### What is Bedrock?

Bedrock is a modern WordPress stack that helps you get started with the best development tools and project structure.

Much of the philosophy behind Bedrock is inspired by the [Twelve-Factor App](http://12factor.net/) methodology including the [WordPress specific version](https://roots.io/twelve-factor-wordpress/).

Read more about [Bedrock](https://roots.io/bedrock/)

### What is Trellis?

Trellis is a set of [Ansible](http://www.ansible.com/home) [playbooks](http://docs.ansible.com/playbooks.html) to automatically configure servers and deploy WordPress sites. It easily creates development environments with Vagrant to help achieve development & production parity.

## Requirements for both Bedrock and Trellis

In order to run this project on your machine, you will need to have the following dependencies installed on your machine. It is important that these dependencies are installed before proceeding to get Bedrock and trellis. It is possible that you have some of these dependencies already installed on your machine, in that case just skip the particular dependency and move on. This tutorial is heavily focussed on an Ubuntu platform, however, the majority of the steps are very similar for Mac. There are certain dependencies that are currently not supported on a Windows platform, as such, if you are on a Windows platform, please look at all the requirements before proceeding with the installation process.

Note: You will require at least 5GB of disk space to successfully install requirements and the virtual machine.

You will be able to install most dependencies without the need of sudo, but some dependencies will need you to use sudo. The majority of the requirements will be installed from the terminal, so whenever you read ‘type this command’ or anything similar, this refers to typing the command into the terminal.

#### Python library

You want to check that you have python-software-properties installed. This will help us add PPAs:

`$ sudo apt-get install python-software-properties`

#### Install any editor

You can use your favourite editor.

#### PHP

* `$ sudo add-apt-repository ppa:ondrej/php5`
* `$ sudo apt-get update`
* `$ sudo apt-get install php5`

#### PHP FPM

Since we are using Nginx, we use PHP FPM to allow it to support PHP:

`$ sudo apt-get install php5-fpm`

#### MariaDB

Now that Oracle owns MySQL, MariaDB was developed as a “drop-in replacement” for MySQL. For now, this means that you can have MySQL or MariaDB installed and when actually using the database, you would not be able to tell the difference. They look exactly alike. You can read about the benefits of MariaDB over MySQL in a variety of places on the Internet. To get mariaDB, you will need to go to the download site:
[MariaDB](https://downloads.mariadb.org/mariadb/repositories/#mirror=digitalocean-lon)

For Ubuntu only: select your version of OS, we’re using Ubuntu and we used the following to install mariaDB:

* `sudo apt-get install software-properties-common`
* `sudo apt-key adv --recv-keys --keyserver hkp://keyserver.ubuntu.com:80 0xcbcb082a1bb943db`
* `sudo add-apt-repository 'deb (http://lon1.mirrors.digitalocean.com/mariadb/repo/10.0/ubuntu) trusty main'`
Once the key is imported and the repository added you can install MariaDB with:

* `sudo apt-get update`
* `sudo apt-get install mariadb-server`

To check that you have mariaDB installed type in the terminal:  
* `mysql --version`

#### Nginx
This is a high performance http server, an alternative to Apache. You can read more about it here: [Nginx](http://wiki.nginx.org/Main)

Install
* `sudo add-apt-repository ppa:nginx/stable`
* `sudo apt-get update`
* `sudo apt-get install nginx`

to check version:
* `nginx -v`

#### Composer
This is the php counterpart of npm in Node and Bundler for Ruby. Bedrock uses Composer to pull in all of the dependencies for a WordPress project together.

Install
* `curl -sS http://getcomposer.org/installer | php`
* `sudo mv composer.phar /usr/local/bin/composer`

Check version:

* `composer --version`

#### Ansible
This is an automation tool for configuration and deployment. You can read more about what ansible is here:[Ansible](http://www.ansible.com/how-ansible-works)

To get ansible, go to the website: http://docs.ansible.com/ansible/intro_installation.html#latest-releases-via-apt-ubuntu, you need to make sure that you download the latest

For ubuntu:

* `sudo apt-get install software-properties-common`
* `sudo apt-add-repository ppa:ansible/ansible`
* `sudo apt-get update`
* `sudo apt-get install ansible`

#### Virtual box
For the virtual machine to be installed on your computer, you will need to download and install the latest version of VirtualBox. As of this writing, the latest version is 5.0. Make sure that you have 5.0 or greater on your machine.

Go to https://www.virtualbox.org/wiki/Downloads and choose your OS version.

#### Vagrant
Vagrant is used to build a development environment on top of the virtual box. To get Vagrant go to the download website: http://www.vagrantup.com/downloads.html. Make sure you install the latest version. 1.7.4 or greater.

Check version by typing: 
* `vagrant --version`

#### vagrant-bindfs
To install type:
* `vagrant plugin install vagrant-bindfs`

#### vagrant-hostsupdater
To install type:
* `vagrant plugin install vagrant-hostsupdater`

#### NFS
Make sure you have nfs-common installed. We're not sure if other OSs will need this, but vagrant will not run without it on ubuntu. To install type:

* `sudo apt-get install nfs-common`

Just before running the install for the virtual machine, you may get an error similar to: 
* `/etc/init.d/nfs-kernel-server`. 
To prevent this error, type:
* `sudo apt-get install nfs-kernel-server nfs-common portmap`
This will set up up NFS on your machine for you.

#### Setup SSH keys for github

Follow instructions here: (https://help.github.com/articles/generating-ssh-keys/)

# Up to this point you should have all generic dependencies installed on your machine! 


## Example Project using Bedrock, Trellis and Sage

## Install Trellis

Installing Trellis is the first thing that needs to be done. Make a new directory for your project on your machine and cd into it:

#### Instructions

1. Create a new project directory: 
`$ mkdir example.com && cd example.com`

2. Clone Trellis: 
`$ git clone --depth=1 git@github.com:roots/trellis.git trellis && rm -rf ansible/.git`

3. Clone Bedrock: 
`$ git clone --depth=1 git@github.com:roots/bedrock.git site && rm -rf site/.git`

4. Clone Sage: 
`$ git clone --depth=1 git@github.com:roots/sage.git site/web/app/themes/sage && rm -rf site/web/app/themes/sage/.git`

5. Move Vagrantfile to root:
`$ mv trellis/Vagrantfile .`

6. Update the ANSIBLE_PATH to `File.join(__dir__, 'trellis')`. You can find the ansible path in `trellis/config/application.php`

After that your folder structure is complete and you're ready to configure the individual components.

### Bedrock 

Bedrock doesn't need any additional configuration by default. There's only one custom option set in this example: WP_DEFAULT_THEME.

* in `site/config/application.php` add `define('WP_DEFAULT_THEME', 'sage');`

### Trellis

Trellis' instructions apply here, but more specifically:

1. Install the Ansible Galaxy roles, from your root dir run: 
* `$ cd site && ansible-galaxy install -r requirements.yml`
2. To be able to configure wordpress_site docs you need to create a database on your machine.

Create a database and a user. You need to have mysql set up. Run mysql on a seperate terminal.

To loginto mysql type:
* `mysql -u root -p`

Then enter your root password. Then you will need to do the following:
* Create a new database by typing: `CREATE DATABASE database name;`
* Create a user and give password: `CREATE USER 'newuser'@'localhost' IDENTIFIED BY 'password';`
* Grant the user all privileges on all databases on MySql: `GRANT ALL PRIVILEGES ON * . * TO 'newuser'@'localhost';` (optionally you can grant privilage to specific database)

2. Configure your wordpress_site file. You can find this file in 

* `trellis/group_vars/development`

This is a file that will be used by trellis to install WordPress. At minimum you will need to enter a site name, database, user and password. You can get more info here: (https://github.com/roots/trellis#wordpress-sites)
In our case we will we will enter some basic site info under wordpress_sites block:
* `example.com:`
  * `site_hosts:`
    * `- example.dev`
    * `local_path: '../site'` # path targeting local Bedrock project directory (relative to Ansible root)
    * `repo: git@github.com:roots/bedrock.git`
    * `site_install: true`
    * `site_title: Example Title`
    * `admin_user: admin`
    * `admin_password: admin`
    * `admin_email: admin@example.dev`
    * `multisite:`
      * `enabled: false`
      * `subdomains: false`
    * `ssl:`
      * `enabled: false`
    * `system_cron: true`
    * `env:`
      * `wp_home: http://example.dev`
      * `wp_siteurl: http://example.dev/wp`
      * `wp_env: development`
      * `db_name: name of the database`
      * `db_user: user`
      * `db_password: password`  

### Sage starter theme

Sage is a starter theme framework from the guys at roots.io (https://github.com/roots/sage), for the example website we will be using the Sage starter theme as a base framework for custom development. We need to install this theme and its dependencies. In order to use the theme framework, you will need to have:

- PHP >= 5.4.x
- Node.js 0.12.x [Install](https://nodejs.org/) 
- npm
- gulp >= 3.8.10 Type: `npm install -g gulp`
- Bower >= 1.3.12 Type: `npm install -g bower`

Please make sure that you have these installed before proceeding.

Next we need to add a plugin called soil. You can do this by going into the site directory and running the following command:
`composer require roots/soil`

You can configure and customize the theme as you please, you can follow more instructions here: (https://github.com/roots/sage#theme-development)

Now its time to install the theme, to do so, you will need to go to the theme directory 
`cd site/web/app/themes/sage` and run the following commands:

* `npm install`
* `bower install`
* `gulp`

## Lift off

If you have followed the steps up to this point and have installed all of the requirements, you are ready to start up the virtual machine. Navigate to your root directory where your Vagrantfile is located and type:

vagrant up

## Possible errors

If you get an error that says something to the effect of: 
###### "The following SSH command responded with a non-Zero status"
then you need to go into the guest machine by typing:

`vagrant ssh`

Then cd into the etc directory by typing:

`cd /etc`

You will then need to edit a file called `sudoers`. It's quickest to do this straight from the terminal and you will need sudo, just type:

`sudo nano sudoers` - nano is a ubuntu editor - you can use any editor

Then add the following two lines to the file:

* `VAGRANT ALL(ALL) NOPASSWD:ALL`
* `Defaults:vagrant !requiretty`

Then exit guest machine by typing:

* `exit`

Then reload vagrant again by typing:

* `vagrant reload`

Once you enter vagrant reload, it will take some time to fire up the virtual machine, just sit tight, maybe make some coffee etc. The installation should complete with no error.

Go to url in our case it’s: example.dev. You should see the default WordPress installation along with the Sage starter theme as the default theme. You can log into the admin section by typing: example.dev/wp/wp-admin - and enter the user and password as specified in your development file.

If you encounter any issues when running vagrant up, please raise an issue in this repo and we will try our best to help you out.
