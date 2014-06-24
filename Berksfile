#!/usr/bin/env ruby
# ^syntax detection

source "https://api.berkshelf.com"

# System setup
cookbook 'apt'
cookbook 'apt-periodic'
cookbook 'build-essential'
cookbook 'sysctl'#,
  # :git => 'https://github.com/spheromak/sysctl-cookbook',
  # :ref => '796678a25e1c83f7f5810c1b507acaedd48403c9'

# System services
cookbook 'cron'#,
  # :git => 'https://github.com/opscode-cookbooks/cron',
  # :ref => '8007fc9ef6083e04554d729b4ffaef4e7ca81d7a'
cookbook 'firewall'
cookbook 'hollandbackup',
  # Because https://github.com/escapestudios-cookbooks/hollandbackup/pull/3
  :git => 'https://github.com/brint/hollandbackup'
cookbook 'monit',
  # Because untagged release 0.7.3 + fixes
  :git => 'https://github.com/apsoto/monit'

# User Services
cookbook 'memcached'
cookbook 'mysql'
cookbook 'varnish'
cookbook 'apache2',
  # For apache 2.4 support needed with ubuntu 14.04
  :git => 'https://github.com/onehealth-cookbooks/apache2-onehealth'
cookbook 'php'
cookbook 'php-fpm'

