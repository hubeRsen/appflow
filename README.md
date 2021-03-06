# AppFlow

[![Join the chat at https://gitter.im/ttssdev/appflow](https://badges.gitter.im/ttssdev/appflow.svg)](https://gitter.im/ttssdev/appflow?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

AppFlow is a multitenant environment automation tool based on Ansible.

`% make provision env=production limit=webservers tenant=mrrobot tags=base_packages`

## Features

Provisiong:

* Multitenant architecture (different teams with different environments)
* Supports `development`, `testing`, `staging` and `production`
* All configuration files are encrypted in git with `ansible-vault`
* Provision all nodes with one command

Development:

* Provides a Vagrant based development environment called `atlantis`
* Code locally on any Unix-like system or Windows (cygwin)

Deployment:

* Made for [Bedrock](https://roots.io/bedrock/) projects and [bedrock-capistrano](https://github.com/roots/bedrock-capistrano) deployments
* Deploy and rollback with one command

Infrastructure:

* Builtin [Percona XtraDB Cluster 5.6](https://www.percona.com/software/mysql-database/percona-xtradb-cluster) and [GlusterFS](http://www.gluster.org) support for sharing web uploads on multiple nodes
* Easy development environments with [Vagrant](http://www.vagrantup.com/)
* Easy server provisioning with [Ansible](http://www.ansible.com/) (Ubuntu 14.04, PHP 5.6)

## Requirements

* `% brew install md5sha1sum`

### Capistrano

#### Ubuntu

```
% sudo apt-get install software-properties-common
% sudo apt-add-repository ppa:brightbox/ruby-ng
% sudo apt-get update
% sudo apt-get install ruby2.3 libxml2-dev zlib1g-dev
% sudo gem2.3 install bundler
```

## Installation

1. Create new config directory layout - `mkdir -p ~/.appflow/{tenant,vault}`
2. Copy `config.example` to `~/.appflow/config` and update variables:
  * `CFG_TENANT_ID` - identity of the own tenant
  * `CFG_TENANT_NAME` - name of the own tenant
  * `CFG_TENANT_ENV` - default provisioning environment
3. Copy the `appflow-mrrobot` folder in `examples` to `~/.appflow/tenant/`
4. Link the folder: `ln -s ~/.appflow/tenant/appflow-mrrobot ~/.appflow/tenant/mrrobot`
5. Copy the `vault` folder in `examples` to `~/.appflow/vault`

Now you are ready to deploy!
In the main appflow folder (where you cloned the repo) you can start provisioning with:
	`make provision env=development tenant=mrrobot`

## Documentation

For easy code management, just use:

```
% make checkout env=production tenant=mrrobot
% make decrypt env=production tenant=mrrobot
% edit tenant's configs in ~/.appflow/tenant/appflow-mrrobot/production
% make status env=production tenant=mrrobot
% make checkin env=production tenant=mrrobot
```

Forgot what you've done? go back:

`% make reset env=production tenant=mrrobot`

Want to update everything and provision?

`% make update ; make checkout ; make provision local=true`

## Tags

`% make tags`

```
play #1 (all): all	TAGS: []
    TASK TAGS: [apache2, apache2-conf, apt, apt-listchanges, apticron, base_packages, borg, borgmatic, cloud, clustercheck, common, composer, env, etckeeper, fstab, geoip, glusterfs, groups, haproxy, haproxy-acl, haproxy-conf, hosts, htaccess, jenkins, keepalived, keepalived-conf, lvm, motd, mysql, mysql-conf, mysql-users, mysqlpass, nodejs, ntp, nullmailer, percona, php, php-conf, pkg, rsyslog, shell, shell-users, smtpd, ssh, ssl, ssl-conf, sudo, swap, update, users, varnish, varnish-conf, vhosts, web_packages, wp-cli, xfs]
```

## Vagrant

```
% vagrant plugin install vagrant-vbguest
% vagrant vbguest --status
```

Before you can `vagrant up atlantis`. This will download the needed trusty64 box.
```
% make vagrant
```

### Troubleshooting

```
Issue: The box you attempted to add doesn't match the provider you specified.`
Solve: % vagrant up --provider=virtualbox atlantis
```

Issue: Lost Vagrant reference to VirtualBox VM
Solve:
```
% VBoxManage list vms
  "vagrant-atlantis" {xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxx}
% echo xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxx > ~/appflow/.vagrant/machines/atlantis/virtualbox/id
```

Issue: `vagragnt Warning: Authentication failure. Retrying...`
Solve: http://stackoverflow.com/a/30792296

Issue: `An error occurred while downloading the remote file. The error message, if any, is reproduced below. Please fix this error and try again.`
Solve: `sudo mv /opt/vagrant/embedded/bin/curl /tmp` https://github.com/mitchellh/vagrant/issues/7997

### Developers

`ansible all -m setup --tree /tmp/facts -i examples/appflow-mrrobot/local/inventory -a "filter=ansible_distribution*"`

## Contributing

Contributions are welcome from everyone. [Join the chat](https://gitter.im/ttssdev/appflow?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge).

## Community

Keep track of development and community news.

TODO.
