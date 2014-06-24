# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.require_version ">= 1.5.0"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  # Default image
  config.vm.box = "opscode_ubuntu-14.04_chef-provisionerless"
  config.vm.box_url = "http://opscode-vm-bento.s3.amazonaws.com/vagrant/virtualbox/opscode_ubuntu-14.04_chef-provisionerless.box"

  # Configure Rackspace provider
  config.vm.provider :rackspace do |rs, override|

    # Rackspace authentication
    rs.username         = ENV['OS_USERNAME']
    rs.api_key          = ENV['NOVA_OS_PASSWORD']

    # Rackspace region
    # This can be any of dfw, :ord, :lon, :iad, :syd
    rs.rackspace_region = :dfw

    # The public key to use for ssh connections. Breaks with large keys.
    # Use key_name instead to use key already uploaded to the rackspace
    # cloud control panel.
    # rs.public_key_path  = "~/.ssh/id_rsa.pub"
    rs.key_name         = 'my-key-name'


    # The image to use for these boxes.
    # Look these up with `vagrant rackspace images list`
    # https://github.com/mitchellh/vagrant-rackspace/issues/109
    # rs.image            = /Ubuntu 14.04 LTS (Trusty Tahr) (PVHVM)/
    rs.image            = 'bb02b1a3-bc77-4d17-ab5b-421d89850fca'

    # Override the default box
    override.vm.box     = "dummy"
    override.vm.box_url = "https://github.com/mitchellh/vagrant-rackspace/raw/master/dummy.box"
  end

  # Configure the database server
  config.vm.define "data" do |data|

    # Box specific rackspace configuration
    data.vm.provider :rackspace do |rs|

      # Server flavor
      # rs.flavor     = /1 GB Performance/
      rs.flavor     = 'performance1-1'
      rs.server_name = 'data'

      # Optional metadata
      # rs.metadata = {"key" => "value"}

      # Attach to an existing network
      # rs.network 'xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'

    end

    # Server name
    data.vm.hostname = "data"

    # Set the version of chef to install using the vagrant-omnibus plugin
    data.omnibus.chef_version = :latest

    # Network configuration
    # data.vm.network :public_network, bridge: 'en1: Wi-Fi (AirPort)'
    data.vm.network :private_network, ip: "10.184.100.80"
    data.vm.network :private_network, ip: "192.168.42.10"

    # Berkshelf configuration
    # data.berkshelf.berksfile_path = "./Berksfile"
    data.berkshelf.enabled = true
    # data.berkshelf.only = []
    # data.berkshelf.except = []

    # Chef configuration
    data.vm.provision :chef_solo do |chef|

      chef.json = {
        hollandbackup: {
          main: {
            backup_directory: "/var/lib/mysqlbackup",
            mysqldump: {
              user: "root",
              host: "localhost",
              password: "rootpass"
            }
          }
        },
        memcached: {
          memory: 256,
          listen: '192.168.42.10'
        },
        mysql: {
          bind_address: '192.168.42.10',
          server_root_password: 'rootpass',
          server_debian_password: 'debpass',
          server_repl_password: 'replpass',
          remove_test_database: true,
          remove_anonymous_users: true,
          root_network_acl: [ "192.168.42.%" ]
        }
      }

      chef.run_list = [
        "recipe[apt]",
        "recipe[apt-periodic]",
        "recipe[build-essential]",
        "recipe[mysql::server]",
        "recipe[memcached]",
        "recipe[hollandbackup]",
        "recipe[hollandbackup::mysqldump]",
        "recipe[hollandbackup::main]",
        "recipe[hollandbackup::backupsets]",
        "recipe[hollandbackup::cron]"
      ]
    end
  end

  # Configure the web server
  config.vm.define "web" do |web|

    # Box specific rackspace configuration
    web.vm.provider :rackspace do |rs|

      # Server flavor
      # rs.flavor     = /2 GB Performance/
      rs.flavor     = 'performance1-2'

      # Server Name
      rs.server_name = 'web'

      # Optional metadata
      # rs.metadata = {"key" => "value"}

      # Attach to an existing network
      # rs.network 'xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'

    end

    # Server name
    web.vm.hostname = 'web'

    # Set the version of chef to install using the vagrant-omnibus plugin
    web.omnibus.chef_version = :latest

    # Network configuration
    # web.vm.network :public_network, bridge: 'en1: Wi-Fi (AirPort)'
    web.vm.network :private_network, ip: "10.184.100.81"
    web.vm.network :private_network, ip: "192.168.42.11"

    # Berkshelf configuration
    # web.berkshelf.berksfile_path = "./Berksfile"
    web.berkshelf.enabled = true
    # web.berkshelf.only = []
    # web.berkshelf.except = []

    # Chef configuration
    web.vm.provision :chef_solo do |chef|
      chef.json = {
        monit: {
          notify_email: 'root@localhost',
          mail_format: {
            from: 'monit@localhost'
          }
        },
        sysctl: {
          params: {
            fs: {
              inotify: {
                max_user_watches: 1000000
              }
            }
          }
        },
        mysql: {
          bind_address: '192.168.42.11',
          server_root_password: 'rootpass'
        },
        "php-fpm" => {
          pools: [
            {
              name: 'default'
            },
            {
              name: 'www'
            }
          ]
        },
        apache: {
          listen_ports: [8080],
          mpm: 'worker',
          default_modules: [
            "status",
            "alias",
            "auth_basic",
            "authn_core",
            "authn_file",
            "authz_core",
            "authz_groupfile",
            "authz_host",
            "authz_user",
            "autoindex",
            "dir",
            "env",
            "mime",
            "negotiation",
            "setenvif",
            "rewrite",
            "proxy_fcgi"
          ]
        },
        varnish: {
          version: "4.0",
          listen_port: "80"
        }
      }

      chef.run_list = [
        "recipe[apt]",
        "recipe[apt-periodic]",
        "recipe[build-essential]",
        "recipe[sysctl::apply]",
        "recipe[mysql::client]",
        # "recipe[mysql-chef_gem]",
        "recipe[php]",
        "recipe[php-fpm]",
        "recipe[apache2]",
        "recipe[varnish]"
      ]
    end
  end

end
