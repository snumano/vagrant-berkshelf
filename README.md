1.repository作成 & git
pochi-2:Vagrant snumano$ mkdir Ubuntu6
pochi-2:Vagrant snumano$ cd Ubuntu6
pochi-2:Ubuntu6 snumano$ ls
pochi-2:Ubuntu6 snumano$ berks cookbook chef-repo
      create  chef-repo/files/default
      create  chef-repo/templates/default
      create  chef-repo/attributes
      create  chef-repo/definitions
      create  chef-repo/libraries
      create  chef-repo/providers
      create  chef-repo/recipes
      create  chef-repo/resources
      create  chef-repo/recipes/default.rb
      create  chef-repo/metadata.rb
      create  chef-repo/LICENSE
      create  chef-repo/README.md
      create  chef-repo/Berksfile
      create  chef-repo/Thorfile
      create  chef-repo/chefignore
      create  chef-repo/.gitignore
         run  git init from "./chef-repo"
      create  chef-repo/Gemfile
      create  chef-repo/Vagrantfile
pochi-2:Ubuntu6 snumano$ cd chef-repo/
pochi-2:chef-repo snumano$ ls -p
Berksfile Thorfile  definitions/  providers/
Gemfile   Vagrantfile files/    recipes/
LICENSE   attributes/ libraries/  resources/
README.md chefignore  metadata.rb templates/
pochi-2:chef-repo snumano$ git init 
Reinitialized existing Git repository in /Users/snumano/Documents/Vagrant/Ubuntu6/chef-repo/.git/
pochi-2:chef-repo snumano$ git add .
pochi-2:chef-repo snumano$ git commit -m 'init'
[master (root-commit) 90ff053] init
 10 files changed, 240 insertions(+)
 create mode 100644 .gitignore
 create mode 100644 Berksfile
 create mode 100644 Gemfile
 create mode 100644 LICENSE
 create mode 100644 README.md
 create mode 100644 Thorfile
 create mode 100644 Vagrantfile
 create mode 100644 chefignore
 create mode 100644 metadata.rb
 create mode 100644 recipes/default.rb
pochi-2:chef-repo snumano$ 


2. .gitignore準備
pochi-2:chef-repo snumano$ cat .gitignore 
.vagrant
Berksfile.lock
*~
*#
.#*
\#*#
.*.sw[a-z]
*.un~
/cookbooks

# Bundler
Gemfile.lock
bin/*
.bundle/*


3.Vagrantfile準備
pochi-2:chef-repo snumano$ vi Vagrantfile 
pochi-2:chef-repo snumano$ cat Vagrantfile
# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  # Every Vagrant virtual environment requires a box to build off of.
  config.vm.box = "precise64"
  config.vm.box_url = "http://files.vagrantup.com/precise64.box"
  config.vm.network :forwarded_port, guest: 80, host: 8080

  #src_dir = './sandbox'
  #doc_root = '/vagrant_data'
  #config.vm.synced_folder src_dir, doc_root

  config.vm.hostname = "precise64"

  # The path to the Berksfile to use with Vagrant Berkshelf
  # config.berkshelf.berksfile_path = "./Berksfile"

  # Enabling the Berkshelf plugin. To enable this globally, add this configuration
  # option to your ~/.vagrant.d/Vagrantfile file
  config.berkshelf.enabled = true

  # An array of symbols representing groups of cookbook described in the Vagrantfile
  # to exclusively install and copy to Vagrant's shelf.
  # config.berkshelf.only = []

  # An array of symbols representing groups of cookbook described in the Vagrantfile
  # to skip installing and copying to Vagrant's shelf.
  # config.berkshelf.except = []

  config.vm.provision :chef_solo do |chef|
  #   chef.cookbooks_path = ["./cookbooks", "./site-cookbooks"]
  #   chef.roles_path = "./roles"
  #   chef.data_bags_path = "../my-recipes/data_bags"
    chef.add_recipe "apt"
    chef.add_recipe "nginx"
    chef.add_recipe "mysql::server"
    chef.add_recipe "mysql"
    chef.add_recipe "database::mysql"
    chef.add_recipe "chef-repo::db"

    chef.json = {
      "mysql" => {
        "server_root_password" => "hello",
        "server_repl_password" => "hello",
        "server_debian_password" => "hello"
      }
    }
  end
end

4.Gemfile確認 & bundle実行
pochi-2:chef-repo snumano$ cat Gemfile
source 'https://rubygems.org'

gem 'berkshelf'

pochi-2:chef-repo snumano$ bundle
Fetching gem metadata from https://rubygems.org/........
Fetching gem metadata from https://rubygems.org/..
Resolving dependencies...
Using i18n (0.6.1) 
Using multi_json (1.7.7) 
Using activesupport (3.2.13) 
Using addressable (2.3.5) 
Using builder (3.2.2) 
Using gyoku (1.0.0) 
Using nokogiri (1.5.10) 
Using akami (1.2.0) 
Using buff-ruby_engine (0.1.0) 
Using buff-shell_out (0.1.0) 
Using timers (1.1.0) 
Using celluloid (0.14.1) 
Using hashie (2.0.5) 
Using chozo (0.6.1) 
Using multipart-post (1.2.0) 
Using faraday (0.8.7) 
Using minitar (0.5.4) 
Using rbzip2 (0.2.0) 
Using retryable (1.3.3) 
Using buff-extensions (0.5.0) 
Using nio4r (0.4.6) 
Using celluloid-io (0.14.1) 
Using erubis (2.7.0) 
Installing json (1.8.0) 
Using mixlib-log (1.6.0) 
Using mixlib-authentication (1.3.0) 
Using net-http-persistent (2.8) 
Using net-ssh (2.6.8) 
Using solve (0.6.1) 
Installing varia_model (0.1.1) 
Using ffi (1.9.0) 
Using gssapi (1.0.3) 
Using httpclient (2.2.0.2) 
Using little-plugger (1.1.3) 
Using logging (1.6.2) 
Using rubyntlm (0.1.1) 
Using rack (1.5.2) 
Using httpi (0.9.7) 
Using nori (1.1.5) 
Using wasabi (1.0.0) 
Using savon (0.9.5) 
Using uuidtools (2.1.4) 
Using winrm (1.1.2) 
Using ridley (1.2.4) 
Using thor (0.18.1) 
Using berkshelf (2.0.7) 
Using bundler (1.3.5) 
Your bundle is complete!
Use `bundle show [gemname]` to see where a bundled gem is installed.


5.Berksfile準備 & berks install実行
pochi-2:chef-repo snumano$ vi Berksfile 
pochi-2:chef-repo snumano$ cat Berksfile 
site :opscode

cookbook 'apt', '= 1.9.0'
cookbook 'mysql'
cookbook 'nginx'
cookbook 'database'


6.metadata.rbにdepends行を追加 & db.rb 編集
pochi-2:chef-repo snumano$ ls
Berksfile Gemfile.lock  Vagrantfile definitions providers
Berksfile.lock  LICENSE   Vagrantfile~  files   recipes
Berksfile~  README.md attributes  libraries resources
Gemfile   Thorfile  chefignore  metadata.rb templates
pochi-2:chef-repo snumano$ vi metadata.rb 
pochi-2:chef-repo snumano$ cat metadata.rb 
name             'chef-repo'
maintainer       'YOUR_NAME'
maintainer_email 'YOUR_EMAIL'
license          'All rights reserved'
description      'Installs/Configures chef-repo'
long_description IO.read(File.join(File.dirname(__FILE__), 'README.md'))
version          '0.1.0'
depends 'database'

pochi-2:chef-repo snumano$ ls
Berksfile Gemfile.lock  Vagrantfile definitions providers
Berksfile.lock  LICENSE   Vagrantfile~  files   recipes
Berksfile~  README.md attributes  libraries resources
Gemfile   Thorfile  chefignore  metadata.rb templates
pochi-2:chef-repo snumano$ cd recipes/
pochi-2:recipes snumano$ ls
default.rb  default.rb~
pochi-2:recipes snumano$ vi db.rb
pochi-2:recipes snumano$ 
pochi-2:recipes snumano$ 
pochi-2:recipes snumano$ cat db.rb 
include_recipe "database::mysql"

# Store this in a variable so we don't keep repeating it
mysql_connection_info = {
    :host => "localhost",
    :username => 'root',
    # automatically get this from the override_attributes call!
    :password => node['mysql']['server_root_password']
}

# my_database = database name
mysql_database 'my_database' do
  connection mysql_connection_info
  action :create
end


7.bundle exec vagrant up実行
pochi-2:chef-repo snumano$ bundle exec vagrant up
Bringing machine 'default' up with 'virtualbox' provider...
[default] Importing base box 'precise64'...
[default] Matching MAC address for NAT networking...
[default] Setting the name of the VM...
[default] Clearing any previously set forwarded ports...
[Berkshelf] Updating Vagrant's berkshelf: '/Users/snumano/.berkshelf/default/vagrant/berkshelf-20130722-2818-1s7rnsf-default'
[Berkshelf] Using chef-repo (0.1.0)
[Berkshelf] Using apt (1.9.0)
[Berkshelf] Using mysql (3.0.2)
[Berkshelf] Using nginx (1.7.0)
[Berkshelf] Using database (1.4.0)
[Berkshelf] Using openssl (1.0.2)
[Berkshelf] Using build-essential (1.4.0)
[Berkshelf] Using yum (2.3.0)
[Berkshelf] Using runit (1.1.6)
[Berkshelf] Using ohai (1.1.10)
[Berkshelf] Using postgresql (3.0.2)
[Berkshelf] Using aws (0.101.2)
[Berkshelf] Using xfs (1.1.0)
[default] Creating shared folders metadata...
[default] Clearing any previously set network interfaces...
[default] Preparing network interfaces based on configuration...
[default] Forwarding ports...
[default] -- 22 => 2222 (adapter 1)
[default] -- 80 => 8080 (adapter 1)
[default] Booting VM...
[default] Waiting for VM to boot. This can take a few minutes.
[default] VM booted and ready for use!
[default] Setting hostname...
[default] Configuring and enabling network interfaces...
[default] Mounting shared folders...
[default] -- /vagrant
[default] -- /tmp/vagrant-chef-1/chef-solo-1/cookbooks
[default] Running provisioner: chef_solo...
Generating chef JSON and uploading...
Running chef-solo...
stdin: is not a tty
[2013-07-22T12:36:27+00:00] INFO: *** Chef 10.14.2 ***
[2013-07-22T12:36:27+00:00] INFO: Setting the run_list to ["recipe[apt]", "recipe[nginx]", "recipe[mysql::server]", "recipe[mysql]", "recipe[database::mysql]", "recipe[chef-repo::db]"] from JSON
[2013-07-22T12:36:27+00:00] INFO: Run List is [recipe[apt], recipe[nginx], recipe[mysql::server], recipe[mysql], recipe[database::mysql], recipe[chef-repo::db]]
[2013-07-22T12:36:27+00:00] INFO: Run List expands to [apt, nginx, mysql::server, mysql, database::mysql, chef-repo::db]
[2013-07-22T12:36:27+00:00] INFO: Starting Chef Run for precise64
[2013-07-22T12:36:27+00:00] INFO: Running start handlers
[2013-07-22T12:36:27+00:00] INFO: Start handlers complete.
[2013-07-22T12:36:28+00:00] INFO: ohai plugins will be at: /etc/chef/ohai_plugins
[2013-07-22T12:36:28+00:00] INFO: Processing remote_directory[/etc/chef/ohai_plugins] action create (ohai::default line 30)
[2013-07-22T12:36:28+00:00] INFO: Processing directory[/etc/chef/ohai_plugins] action create (dynamically defined)
[2013-07-22T12:36:28+00:00] INFO: directory[/etc/chef/ohai_plugins] created directory /etc/chef/ohai_plugins
[2013-07-22T12:36:28+00:00] INFO: directory[/etc/chef/ohai_plugins] mode changed to 755
[2013-07-22T12:36:28+00:00] INFO: Processing cookbook_file[/etc/chef/ohai_plugins/README] action create (dynamically defined)
[2013-07-22T12:36:28+00:00] INFO: cookbook_file[/etc/chef/ohai_plugins/README] mode changed to 644
[2013-07-22T12:36:28+00:00] INFO: cookbook_file[/etc/chef/ohai_plugins/README] created file /etc/chef/ohai_plugins/README
[2013-07-22T12:36:28+00:00] INFO: remote_directory[/etc/chef/ohai_plugins] created directory /etc/chef/ohai_plugins
[2013-07-22T12:36:28+00:00] INFO: remote_directory[/etc/chef/ohai_plugins] mode changed to 755
[2013-07-22T12:36:28+00:00] INFO: Processing ohai[custom_plugins] action reload (ohai::default line 44)
[2013-07-22T12:36:28+00:00] INFO: ohai[custom_plugins] reloaded
[2013-07-22T12:36:28+00:00] INFO: Could not find previously defined grants.sql resource
[2013-07-22T12:36:28+00:00] INFO: Processing execute[apt-get-update-build-essentials] action run (build-essential::debian line 22)
[2013-07-22T12:36:39+00:00] INFO: execute[apt-get-update-build-essentials] ran successfully
[2013-07-22T12:36:39+00:00] INFO: Processing package[autoconf] action install (build-essential::debian line 40)
[2013-07-22T12:36:46+00:00] INFO: Processing package[binutils-doc] action install (build-essential::debian line 40)
[2013-07-22T12:36:49+00:00] INFO: Processing package[bison] action install (build-essential::debian line 40)
[2013-07-22T12:36:52+00:00] INFO: Processing package[build-essential] action install (build-essential::debian line 40)
[2013-07-22T12:37:13+00:00] INFO: Processing package[flex] action install (build-essential::debian line 40)
[2013-07-22T12:37:16+00:00] INFO: Processing package[mysql-client] action install (mysql::client line 46)
[2013-07-22T12:37:34+00:00] INFO: Processing package[libmysqlclient-dev] action install (mysql::client line 46)
[2013-07-22T12:37:42+00:00] INFO: Processing chef_gem[mysql] action install (mysql::ruby line 31)
[2013-07-22T12:37:47+00:00] INFO: Processing execute[apt-get-update] action run (apt::default line 22)
[2013-07-22T12:37:53+00:00] INFO: execute[apt-get-update] ran successfully
[2013-07-22T12:37:53+00:00] INFO: Processing execute[apt-get update] action nothing (apt::default line 29)
[2013-07-22T12:37:53+00:00] INFO: Processing execute[apt-get autoremove] action nothing (apt::default line 36)
[2013-07-22T12:37:53+00:00] INFO: Processing execute[apt-get autoclean] action nothing (apt::default line 42)
[2013-07-22T12:37:53+00:00] INFO: Processing package[update-notifier-common] action install (apt::default line 48)
[2013-07-22T12:37:56+00:00] INFO: package[update-notifier-common] sending run action to execute[apt-get-update] (immediate)
[2013-07-22T12:37:56+00:00] INFO: Processing execute[apt-get-update] action run (apt::default line 22)
[2013-07-22T12:38:01+00:00] INFO: execute[apt-get-update] ran successfully
[2013-07-22T12:38:01+00:00] INFO: Processing execute[apt-get-update-periodic] action run (apt::default line 52)
[2013-07-22T12:38:01+00:00] INFO: Processing directory[/var/cache/local] action create (apt::default line 62)
[2013-07-22T12:38:01+00:00] INFO: directory[/var/cache/local] created directory /var/cache/local
[2013-07-22T12:38:01+00:00] INFO: directory[/var/cache/local] owner changed to 0
[2013-07-22T12:38:01+00:00] INFO: directory[/var/cache/local] group changed to 0
[2013-07-22T12:38:01+00:00] INFO: directory[/var/cache/local] mode changed to 755
[2013-07-22T12:38:01+00:00] INFO: Processing directory[/var/cache/local/preseeding] action create (apt::default line 62)
[2013-07-22T12:38:01+00:00] INFO: directory[/var/cache/local/preseeding] created directory /var/cache/local/preseeding
[2013-07-22T12:38:01+00:00] INFO: directory[/var/cache/local/preseeding] owner changed to 0
[2013-07-22T12:38:01+00:00] INFO: directory[/var/cache/local/preseeding] group changed to 0
[2013-07-22T12:38:01+00:00] INFO: directory[/var/cache/local/preseeding] mode changed to 755
[2013-07-22T12:38:01+00:00] INFO: Processing ohai[reload_nginx] action nothing (nginx::ohai_plugin line 22)
[2013-07-22T12:38:01+00:00] INFO: Processing template[/etc/chef/ohai_plugins/nginx.rb] action create (nginx::ohai_plugin line 27)
[2013-07-22T12:38:02+00:00] INFO: template[/etc/chef/ohai_plugins/nginx.rb] updated content
[2013-07-22T12:38:02+00:00] INFO: template[/etc/chef/ohai_plugins/nginx.rb] owner changed to 0
[2013-07-22T12:38:02+00:00] INFO: template[/etc/chef/ohai_plugins/nginx.rb] group changed to 0
[2013-07-22T12:38:02+00:00] INFO: template[/etc/chef/ohai_plugins/nginx.rb] mode changed to 755
[2013-07-22T12:38:02+00:00] INFO: template[/etc/chef/ohai_plugins/nginx.rb] sending reload action to ohai[reload_nginx] (immediate)
[2013-07-22T12:38:02+00:00] INFO: Processing ohai[reload_nginx] action reload (nginx::ohai_plugin line 22)
[2013-07-22T12:38:02+00:00] INFO: ohai[reload_nginx] reloaded
[2013-07-22T12:38:02+00:00] INFO: Processing remote_directory[/etc/chef/ohai_plugins] action nothing (ohai::default line 30)
[2013-07-22T12:38:02+00:00] INFO: Processing ohai[custom_plugins] action nothing (ohai::default line 44)
[2013-07-22T12:38:02+00:00] INFO: Processing package[nginx] action install (nginx::default line 41)
[2013-07-22T12:38:10+00:00] INFO: Processing service[nginx] action enable (nginx::default line 42)
[2013-07-22T12:38:10+00:00] INFO: Processing directory[/etc/nginx] action create (nginx::commons_dir line 21)
[2013-07-22T12:38:10+00:00] INFO: Processing directory[/var/log/nginx] action create (nginx::commons_dir line 28)
[2013-07-22T12:38:10+00:00] INFO: directory[/var/log/nginx] owner changed to 33
[2013-07-22T12:38:10+00:00] INFO: Processing directory[/etc/nginx/sites-available] action create (nginx::commons_dir line 36)
[2013-07-22T12:38:10+00:00] INFO: Processing directory[/etc/nginx/sites-enabled] action create (nginx::commons_dir line 36)
[2013-07-22T12:38:10+00:00] INFO: Processing directory[/etc/nginx/conf.d] action create (nginx::commons_dir line 36)
[2013-07-22T12:38:10+00:00] INFO: Processing template[/usr/sbin/nxensite] action create (nginx::commons_script line 22)
[2013-07-22T12:38:11+00:00] INFO: template[/usr/sbin/nxensite] updated content
[2013-07-22T12:38:11+00:00] INFO: template[/usr/sbin/nxensite] owner changed to 0
[2013-07-22T12:38:11+00:00] INFO: template[/usr/sbin/nxensite] group changed to 0
[2013-07-22T12:38:11+00:00] INFO: template[/usr/sbin/nxensite] mode changed to 755
[2013-07-22T12:38:11+00:00] INFO: Processing template[/usr/sbin/nxdissite] action create (nginx::commons_script line 22)
[2013-07-22T12:38:11+00:00] INFO: template[/usr/sbin/nxdissite] updated content
[2013-07-22T12:38:11+00:00] INFO: template[/usr/sbin/nxdissite] owner changed to 0
[2013-07-22T12:38:11+00:00] INFO: template[/usr/sbin/nxdissite] group changed to 0
[2013-07-22T12:38:11+00:00] INFO: template[/usr/sbin/nxdissite] mode changed to 755
[2013-07-22T12:38:11+00:00] INFO: Processing template[nginx.conf] action create (nginx::commons_conf line 21)
[2013-07-22T12:38:11+00:00] INFO: template[nginx.conf] backed up to /var/chef/backup/etc/nginx/nginx.conf.chef-20130722123811
[2013-07-22T12:38:11+00:00] INFO: template[nginx.conf] updated content
[2013-07-22T12:38:11+00:00] INFO: template[nginx.conf] owner changed to 0
[2013-07-22T12:38:11+00:00] INFO: template[nginx.conf] group changed to 0
[2013-07-22T12:38:11+00:00] INFO: template[nginx.conf] mode changed to 644
[2013-07-22T12:38:11+00:00] INFO: Processing template[/etc/nginx/sites-available/default] action create (nginx::commons_conf line 30)
[2013-07-22T12:38:11+00:00] INFO: template[/etc/nginx/sites-available/default] backed up to /var/chef/backup/etc/nginx/sites-available/default.chef-20130722123811
[2013-07-22T12:38:11+00:00] INFO: template[/etc/nginx/sites-available/default] updated content
[2013-07-22T12:38:11+00:00] INFO: template[/etc/nginx/sites-available/default] owner changed to 0
[2013-07-22T12:38:11+00:00] INFO: template[/etc/nginx/sites-available/default] group changed to 0
[2013-07-22T12:38:11+00:00] INFO: template[/etc/nginx/sites-available/default] mode changed to 644
[2013-07-22T12:38:11+00:00] INFO: template[/etc/nginx/sites-available/default] not queuing delayed action reload on service[nginx] (delayed), as it's already been queued
[2013-07-22T12:38:11+00:00] INFO: Processing execute[nxensite default] action run (nginx::commons_conf line 23)
[2013-07-22T12:38:11+00:00] INFO: Processing service[nginx] action start (nginx::default line 49)
[2013-07-22T12:38:11+00:00] INFO: service[nginx] started
[2013-07-22T12:38:11+00:00] INFO: Processing package[mysql-client] action install (mysql::client line 46)
[2013-07-22T12:38:11+00:00] INFO: Processing package[libmysqlclient-dev] action install (mysql::client line 46)
[2013-07-22T12:38:11+00:00] INFO: Processing directory[/var/cache/local/preseeding] action create (mysql::server line 47)
[2013-07-22T12:38:11+00:00] INFO: Processing execute[preseed mysql-server] action nothing (mysql::server line 54)
[2013-07-22T12:38:11+00:00] INFO: Processing template[/var/cache/local/preseeding/mysql-server.seed] action create (mysql::server line 59)
[2013-07-22T12:38:11+00:00] INFO: template[/var/cache/local/preseeding/mysql-server.seed] updated content
[2013-07-22T12:38:11+00:00] INFO: template[/var/cache/local/preseeding/mysql-server.seed] owner changed to 0
[2013-07-22T12:38:11+00:00] INFO: template[/var/cache/local/preseeding/mysql-server.seed] group changed to 0
[2013-07-22T12:38:11+00:00] INFO: template[/var/cache/local/preseeding/mysql-server.seed] mode changed to 600
[2013-07-22T12:38:11+00:00] INFO: template[/var/cache/local/preseeding/mysql-server.seed] sending run action to execute[preseed mysql-server] (immediate)
[2013-07-22T12:38:11+00:00] INFO: Processing execute[preseed mysql-server] action run (mysql::server line 54)
[2013-07-22T12:38:12+00:00] INFO: execute[preseed mysql-server] ran successfully
[2013-07-22T12:38:12+00:00] INFO: Processing template[/etc/mysql/debian.cnf] action create (mysql::server line 67)
[2013-07-22T12:38:12+00:00] INFO: template[/etc/mysql/debian.cnf] updated content
[2013-07-22T12:38:12+00:00] INFO: template[/etc/mysql/debian.cnf] owner changed to 0
[2013-07-22T12:38:12+00:00] INFO: template[/etc/mysql/debian.cnf] group changed to 0
[2013-07-22T12:38:12+00:00] INFO: template[/etc/mysql/debian.cnf] mode changed to 600
[2013-07-22T12:38:12+00:00] INFO: Processing package[mysql-server] action install (mysql::server line 94)
[2013-07-22T12:38:56+00:00] INFO: package[mysql-server] sending start action to service[mysql] (immediate)
[2013-07-22T12:38:56+00:00] INFO: Processing service[mysql] action start (mysql::server line 218)
[2013-07-22T12:38:56+00:00] INFO: Processing directory[/var/run/mysqld] action create (mysql::server line 108)
[2013-07-22T12:38:56+00:00] INFO: directory[/var/run/mysqld] group changed to 111
[2013-07-22T12:38:56+00:00] INFO: Processing directory[/var/log/mysql] action create (mysql::server line 108)
[2013-07-22T12:38:56+00:00] INFO: directory[/var/log/mysql] group changed to 111
[2013-07-22T12:38:56+00:00] INFO: Processing directory[/etc/mysql] action create (mysql::server line 108)
[2013-07-22T12:38:56+00:00] INFO: directory[/etc/mysql] owner changed to 106
[2013-07-22T12:38:56+00:00] INFO: directory[/etc/mysql] group changed to 111
[2013-07-22T12:38:56+00:00] INFO: Processing directory[/etc/mysql/conf.d] action create (mysql::server line 108)
[2013-07-22T12:38:56+00:00] INFO: directory[/etc/mysql/conf.d] owner changed to 106
[2013-07-22T12:38:56+00:00] INFO: directory[/etc/mysql/conf.d] group changed to 111
[2013-07-22T12:38:56+00:00] INFO: Processing directory[/var/lib/mysql] action create (mysql::server line 108)
[2013-07-22T12:38:56+00:00] INFO: Processing directory[/var/lib/mysql] action create (mysql::server line 108)
[2013-07-22T12:38:56+00:00] INFO: Processing execute[mysql-install-db] action run (mysql::server line 148)
[2013-07-22T12:38:56+00:00] INFO: Processing service[mysql] action enable (mysql::server line 154)
[2013-07-22T12:38:56+00:00] INFO: Processing execute[assign-root-password] action run (mysql::server line 166)
[2013-07-22T12:38:56+00:00] INFO: Processing template[/etc/mysql/grants.sql] action create (mysql::server line 179)
[2013-07-22T12:38:56+00:00] INFO: template[/etc/mysql/grants.sql] updated content
[2013-07-22T12:38:56+00:00] INFO: template[/etc/mysql/grants.sql] owner changed to 0
[2013-07-22T12:38:56+00:00] INFO: template[/etc/mysql/grants.sql] group changed to 0
[2013-07-22T12:38:56+00:00] INFO: template[/etc/mysql/grants.sql] mode changed to 600
[2013-07-22T12:38:56+00:00] INFO: template[/etc/mysql/grants.sql] sending run action to execute[mysql-install-privileges] (immediate)
[2013-07-22T12:38:56+00:00] INFO: Processing execute[mysql-install-privileges] action run (mysql::server line 195)
[2013-07-22T12:38:56+00:00] INFO: execute[mysql-install-privileges] ran successfully
[2013-07-22T12:38:56+00:00] INFO: Processing execute[mysql-install-privileges] action nothing (mysql::server line 195)
[2013-07-22T12:38:56+00:00] INFO: Processing template[/etc/mysql/my.cnf] action create (mysql::server line 202)
[2013-07-22T12:38:57+00:00] INFO: template[/etc/mysql/my.cnf] backed up to /var/chef/backup/etc/mysql/my.cnf.chef-20130722123857
[2013-07-22T12:38:57+00:00] INFO: template[/etc/mysql/my.cnf] updated content
[2013-07-22T12:38:57+00:00] INFO: template[/etc/mysql/my.cnf] owner changed to 0
[2013-07-22T12:38:57+00:00] INFO: template[/etc/mysql/my.cnf] group changed to 0
[2013-07-22T12:38:57+00:00] INFO: template[/etc/mysql/my.cnf] mode changed to 644
[2013-07-22T12:38:57+00:00] INFO: template[/etc/mysql/my.cnf] sending restart action to service[mysql] (immediate)
[2013-07-22T12:38:57+00:00] INFO: Processing service[mysql] action restart (mysql::server line 218)
[2013-07-22T12:38:59+00:00] INFO: service[mysql] restarted
[2013-07-22T12:38:59+00:00] INFO: Processing service[mysql] action start (mysql::server line 218)
[2013-07-22T12:38:59+00:00] INFO: Processing execute[apt-get-update-build-essentials] action nothing (build-essential::debian line 22)
[2013-07-22T12:38:59+00:00] INFO: Processing package[autoconf] action nothing (build-essential::debian line 40)
[2013-07-22T12:38:59+00:00] INFO: Processing package[binutils-doc] action nothing (build-essential::debian line 40)
[2013-07-22T12:38:59+00:00] INFO: Processing package[bison] action nothing (build-essential::debian line 40)
[2013-07-22T12:38:59+00:00] INFO: Processing package[build-essential] action nothing (build-essential::debian line 40)
[2013-07-22T12:38:59+00:00] INFO: Processing package[flex] action nothing (build-essential::debian line 40)
[2013-07-22T12:38:59+00:00] INFO: Processing chef_gem[mysql] action install (mysql::ruby line 31)
[2013-07-22T12:38:59+00:00] INFO: Processing mysql_database[my_database] action create (chef-repo::db line 12)
[2013-07-22T12:38:59+00:00] INFO: template[nginx.conf] sending reload action to service[nginx] (delayed)
[2013-07-22T12:38:59+00:00] INFO: Processing service[nginx] action reload (nginx::default line 49)
[2013-07-22T12:38:59+00:00] INFO: service[nginx] reloaded
[2013-07-22T12:38:59+00:00] INFO: Chef Run complete in 152.383341 seconds
[2013-07-22T12:38:59+00:00] INFO: Running report handlers
[2013-07-22T12:38:59+00:00] INFO: Report handlers complete
pochi-2:chef-repo snumano$ 


8.bundle exec vagrant sshで接続
pochi-2:chef-repo snumano$ bundle exec vagrant ssh
Welcome to Ubuntu 12.04 LTS (GNU/Linux 3.2.0-23-generic x86_64)

 * Documentation:  https://help.ubuntu.com/

142 packages can be updated.
64 updates are security updates.

Welcome to your Vagrant-built virtual machine.
Last login: Fri Sep 14 06:23:18 2012 from 10.0.2.2
vagrant@precise64:~$ mysql -u root -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 37
Server version: 5.5.31-0ubuntu0.12.04.2-log (Ubuntu)

Copyright (c) 2000, 2013, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| my_database        |
| mysql              |
| performance_schema |
| test               |
+--------------------+
5 rows in set (0.00 sec)

mysql> 


