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
