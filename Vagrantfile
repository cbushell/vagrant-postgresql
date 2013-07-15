# -*- mode: ruby -*-
# vi: set ft=ruby :
HERE = File.join(File.dirname(__FILE__))

Vagrant::Config.run do |config|
  config.vm.box     = "precise64"
  config.vm.box_url = "http://files.vagrantup.com/precise64.box"

  config.vm.define "database" do |cfg|
    cfg.vm.forward_port 5432, 5432    # postgres
    cfg.vm.forward_port 27017, 27017  # mongo
    cfg.vm.forward_port 80, 8080      # phppgadmin
    cfg.vm.provision :chef_solo do |chef|
      chef.cookbooks_path = File.join(HERE, 'cookbooks')
      chef.add_recipe("mongodb::10gen_repo")
      chef.add_recipe("apt")
      chef.add_recipe("postgresql::server")
      chef.add_recipe("phppgadmin")
      chef.add_recipe("mongodb")
      chef.json = {
        :postgresql => {
          :version  => "9.1",
          :listen_addresses => "*",
          :pg_hba => [
            "host all all 0.0.0.0/0 trust",
            "host all all ::1/0 trust",
          ],
          :users => [
            { :username => "postgres", :password => "password",
              :superuser => true, :login => true, :createdb => true }
          ]
        },
        :mongodb => {
            :package_version => "2.2.3"
        }
      }
    end
  end
end
