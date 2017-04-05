# Chronos Puppet Module

[![Puppet
Forge](http://img.shields.io/puppetforge/v/ottode/chronos.svg)](https://forge.puppetlabs.com/ottode/chronos)
[![Build Status](https://travis-ci.org/otto-de/puppet-chronos.svg?branch=master)](https://travis-ci.org/otto-de/puppet-chronos)

Installs and configures the mesos framework chronos.
It plays well with [deric/mesos](https://forge.puppetlabs.com/deric/mesos) puppet module.

## Usage

Install and configure chronos to connect to 3 zookeeper hosts.
Make sure that libmesos.so is installed with the mesos package.

```puppet
class { '::chronos':
  zk_nodes => [
                'zk-host-01:2181',
                'zk-host-02:2181',
                'zk-host-03:2181',
              ],
  require  => Package['mesos'], # require libmesos.so installed
}
```

## Parameters

 - `zk_nodes` - array of zookeeper hosts - mandatory
 - `zk_path_mesos` - zookeeper path for finding mesos master, usually `/mesos`
 - `zk_path_chronos` - zookeeper path for storing chronos state
 - `package` - chronos package name
 - `version` - install specific version of chronos
 - `enable_service` - enable chronos service
 - `options` - additional command line options
 - `java_home` - set JAVA_HOME
 - `run_as_user` - run service under specified user
 - `secret` - secret for connecting to mesos

All of these parameters could be handled by hiera:

```yaml
chronos::zk_nodes:
  - 'zk-host-01:2181'
  - 'zk-host-02:2181'
  - 'zk-host-03:2181'
chronos::zk_path_chronos: '/my/chronos'
```

## Jobs

You can configure chronos jobs by including a `chronos::job` resource:

```puppet
chronos_job{'my-job':
  chronos_url => 'http://localhost:8080',
  api_version => 'v1',
  content => '{ "name": "my-job", "command": "echo hey!", ... }',
}
```

Delete the same job by including:

```puppet
chronos_job{'my-job':
  ensure  => absent,
  chronos_url => 'http://localhost:8080',
  api_version => 'v1',
  content => '{ "name": "my-job" }',
}
```

## Packages

You can build package by yourself and upload package to your software repository. Or use packages from mesosphere.io:

 - RedHat/CentOS
   - [mesosphere packages](http://mesosphere.io/downloads/)

## Requirements

 - Puppet > 3.0 and < 5.0
 - RHEL 7 or CentOS 7

## Dependencies

Chronos needs libmesos.so file installed to run.
This file is __not__ installed by this module.
Use [deric/mesos](https://forge.puppetlabs.com/deric/mesos) to install mesos binaries.

Additional dependencies:

 - [puppetlabs/stdlib](https://forge.puppetlabs.com/puppetlabs/stdlib) version `>= 4.6.0` - we need function `assert_private`

## Installation

Preferred installation is via [puppet-librarian](https://github.com/rodjek/librarian-puppet) just add to `Puppetfile`:

```ruby
mod 'ottode/chronos', '>= 0.1.0'
```

for latest version from git:
```ruby
mod 'ottode/chronos', :git => 'git://github.com/otto-de/puppet-chronos.git'
```

## Links

For more information see [chronos project](https://github.com/mesos/chronos)

## License

Apache License 2.0

## Contributors


Alphabetical list of contributors (not necessarily up-to-date), generated by command `git log --format='%aN' | sort -u | sed -e 's/^/\- /'`:

 - as0bu
 - Felix Bechstein
 - Florian Sellmayr
