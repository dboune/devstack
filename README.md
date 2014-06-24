DevStack
========

This is a template for deploying a simple developer stack. A HEAT template and
Vagrant configuration are included. The Vagrant configuration supports both
VirtualBox and Rackspace. Chef solo is used to setup the servers.

_**THIS TEMPLATE IS IN EARLY DEVELOPMENT**_

HEAT Requirements
=================
* A Heat provider that supports the Rackspace resources as well as the
  `OS::Heat::ChefSolo` plugin.
* An OpenStack username, password, and tenant id.
* [python-heatclient](https://github.com/openstack/python-heatclient)
`>= v0.2.8`:

```bash
pip install python-heatclient
```

Vagrant Requirements
====================
* vagrant-rackspace plugin
* vagrant-omnibus plugin
* vagrant-berkshelf plugin

```bash
vagrant plugin install vagrant-rackspace vagrant-omnibus vagrant-berkshelf
```

HEAT Template Parameters
========================
Parameters can be set using the the `-P` flag with the HEAT client.

* `private_network_cidr`: The CIDR for the private network. (Default:
  192.168.42.0/24)
* `private_network_label`: The label to use for the private network. (Default:
  DevNet)
* `image`: The operating system image to to install on all systems (Default:
  Ubuntu 14.04 LTS (Trusty Tahr) (PVHVM))
* `database_server_flavor`: Cloud server size to use for the database server.
  (Default: 1 GB Performance)
* `web_server_flavor`: Cloud server size to use for the web server. (Default:
  2 GB Performance)
* `kitchen`: URL for the kitchen to clone with git. (Default:
  https://github.com/dboune/devstack)
* `chef_version`: Chef client version to install for the chef run. (Default:
  11.12.8-2)

Contributing
============

Feel free to make a pull request from your feature branch :)

License
=======
```
Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```

All vendored works contianed within the ```cookbooks``` folder are licenced
as specified in the individual package and are only included here in order to
facilitate the use of the `OS::Heat::ChefSolo` HEAT plugin.
