[![Build Status](https://travis-ci.org/hawknewton/puppet-pulp.png?branch=master)](https://travis-ci.org/hawknewton/puppet-pulp)
# Overview
This module makes an honest attempt at installing the
[The Pulp Repository](http://www.pulpproject.org/), including it's consumer and
administration clients, onto a redhat system of your choice.  The Pulp Repo
brings to the table (among other things) the ability to host an internal puppet
forge.

## Quickstart

* Install an [epel repo](http://puppetforge.com/stahnma/epel)
* Disable [selinux](http://puppetforge.com/spiette/selinux)

Install the server, admin client, and consumer on the same box:

```
class { 'pulp': }               # Install pulp v2 yum repo
clsss { 'pulp::server': }       # Install pulp server
class { 'pulp::admin_client': } # Install admin client
class { 'pulp::consumer': }     # Install pulp agent and client
```

## Installation

### Prerequisites
This module needs a few thing to make it go:
* The EPEL repo of your choice.  I tested using `stahnma/epel`, which seems as
  good as any.

* You'll need to disable selinux.

### Usage

#### Class pulp
Installs a local pulp v2 yumrepo.

```
class { 'pulp':
  ensure => 'present'
}

```

##### Parameters
**ensure**
* `enabled`: Install the pulp yumrepo and enable it
* `diabled`: Install and pulp yumrepo and leave it disabled
* `absent`: Remove any yumrepo called `pulp-v2-stable`

If you want to manage the pulp repo yourself, don't include the pulp class.

#### Class pulp::server
Fires up a pulp server, including mongo, qpid, and httpd.

```
class { 'pulp::server':
  ensure => 'present'
}
```

##### Parametres:
**ensure** (defaults to `present`)
* `present`: install the pulp server at the current version
* `2.2.0-1.el6` (for example): pin the pulp server at a specific verion
* `absent`: remove the pulp server, shut down services, and delete config files

**conf_template** (optional)

If provided, use this template instead of the built-in `templates/server.conf.erb`

#### Class pulp::admin_client
Installs and configures the pulp admin client.

```
class { 'pulp::admin_client':
  ensure => 'present'
}
```

##### Parameters:
**ensure** -- optional, defaults to `present`
* `present`: Install the pulp admin client at the current version
* `2.2.0-1.el6` (for example): pin the client to a specific verison
* `absent`: remove the admin client

**server** -- optional, defaults to the current host

**conf_template**  -- optional

If provided, use this template instead of the built-in `templates/admon.conf.erb`

#### Class pulp::consumer
Installs and configures the pulp consumer.
```
class { 'pulp::consumer':
  ensure => 'present'
}
```

##### Parameters:
**ensure** -- optional, defaults to `present`
* `present`: Install the pulp consumer at the current version
* `2.2.0-1.el6` (for example): pin the client ot a specific verison
* `absent`: Remove the pulp-consumer packages

**server** -- optional, defaults to the current host

**conf_template** -- optional

If provided, use this template instead of the built-in `templates/consumer.conf.erb`


## Developing

If you want to issue a pull request to help me out, that'd be awesome.
Make sure you have a fairly recent version of both vagrant and ruby.

```
rake spec spec:system
```
