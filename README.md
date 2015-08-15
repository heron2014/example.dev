# Example.dev

Based on the [Bedrock](https://roots.io/bedrock/) Wordpress development framework, [Trellis](https://github.com/roots/trellis) and [Sage](https://github.com/roots/sage).

## What is Bedrock? 

Bedrock is a modern WordPress stack that helps you get started with the best development tools and project structure.

Much of the philosophy behind Bedrock is inspired by the [Twelve-Factor App](http://12factor.net/) methodology including the [WordPress specific version](https://roots.io/twelve-factor-wordpress/).

Read more about [Bedrock](https://roots.io/bedrock/)

#### Requirements for Bedrock

* PHP >= 5.4
* Composer - [Install](https://getcomposer.org/doc/00-intro.md#installation-linux-unix-osx)

## What is Trellis? 

Trellis is a set of [Ansible](http://www.ansible.com/home) [playbooks](http://docs.ansible.com/playbooks.html) to automatically configure servers and deploy WordPress sites. It easily creates development environments with Vagrant to help achieve development & production parity.

Configure complete Bedrock-based WordPress ready servers with a single command:

|                        | Command
| ---------------------- | ------------------------------------------------ |
| **Development**        | `vagrant up`                                     |
| **Staging/Production** |`ansible-playbook -i hosts/production server.yml` |
| **Deploying**          | `./deploy.sh production <site name>`             |

#### Requirements for Trellis

* Ansible >= 1.9.2 - [Install](http://docs.ansible.com/intro_installation.html) • [Docs](http://docs.ansible.com/) • [Windows wiki](https://github.com/roots/trellis/wiki/Windows)
* Virtualbox >= 4.3.10 - [Install](https://www.virtualbox.org/wiki/Downloads)
* Vagrant >= 1.7.4 - [Install](http://www.vagrantup.com/downloads.html) • [Docs](https://docs.vagrantup.com/v2/)
* vagrant-bindfs >= 0.3.1 - [Install](https://github.com/gael-ian/vagrant-bindfs#installation) • [Docs](https://github.com/gael-ian/vagrant-bindfs) (Windows users may skip this)
* vagrant-hostsupdater - [Install](https://github.com/cogitatio/vagrant-hostsupdater#installation) • [Docs](https://github.com/cogitatio/vagrant-hostsupdater)

Read more about [Trellis](https://github.com/roots/trellis)

#### Other requirements

* [Node](https://nodejs.org/)
* npm module

## How to run Methods Digital Website on your machine

After installing requirements please follow steps below:

1. Clone this repo
2. Create mysql database, user and password on your machine (make a notes of these details)
3. Add Soil: `$ cd site && composer require roots/soil`
4. Navigate to: `site/web/app/themes/sage` and run the following commands in this order:
  * `npm install`
  * `bower install`
  * `gulp`
5. Navigate to your root directory and run: `cd ansible && ansible-galaxy install -r requirements.yml`

## Development setup

1. Configure your `wordpress_sites`: [docs](https://github.com/roots/trellis#wp-sites) in `ansible/group_vars/development` - update your database details from your notes. I.e. the database name, user and password.
2. Navigate to your root directory where your Vagrantfile is located and run: 
`vagrant up`
3. Go to the browser and run : `example.dev`

If you eencounter some issue when running vagrant up please raise an issue and I will try my best to help you out.




