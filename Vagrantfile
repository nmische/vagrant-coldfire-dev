# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant::Config.run do |config|
  # All Vagrant configuration is done here. The most common configuration
  # options are documented and commented below. For a complete reference,
  # please see the online documentation at vagrantup.com.

  # Every Vagrant virtual environment requires a box to build off of.
  config.vm.box = "precise32"

  # The url from where the 'config.vm.box' box will be fetched if it
  # doesn't already exist on the user's system.
  config.vm.box_url = "http://files.vagrantup.com/precise32.box"

  # Boot with a GUI so you can see the screen. (Default is headless)
  # config.vm.boot_mode = :gui

  # Assign this VM to a host-only network IP, allowing you to access it
  # via the IP. Host-only networks can talk to the host machine as well as
  # any other machines on the same network, but cannot be accessed (through this
  # network interface) by any external networks.
  config.vm.network :hostonly, "192.168.33.33"

  # Assign this VM to a bridged network, allowing you to connect directly to a
  # network using the host's network device. This makes the VM appear as another
  # physical device on your network.
  # config.vm.network :bridged

  # Forward a port from the guest to the host, which allows for outside
  # computers to access the VM, whereas host only networking does not.
  # config.vm.forward_port 80, 8080

  # Share an additional folder to the guest VM. The first argument is
  # an identifier, the second is the path on the guest to mount the
  # folder, and the third is the path on the host to the actual folder.
  # config.vm.share_folder "v-data", "/vagrant_data", "../data"

  config.vm.share_folder "v-root", "/vagrant", ".", :owner => "nobody"

  config.vm.customize [
    "modifyvm", :id,
    "--name", "ColdFireDevVM",
    "--memory", "1536"
  ]

  # Enable provisioning with Puppet stand alone.  Puppet manifests
  # are contained in a directory path relative to this Vagrantfile.
  # You will need to create the manifests directory and a manifest in
  # the file base.pp in the manifests_path directory.
  #
  # An example Puppet manifest to provision the message of the day:
  #
  # # group { "puppet":
  # #   ensure => "present",
  # # }
  # #
  # # File { owner => 0, group => 0, mode => 0644 }
  # #
  # # file { '/etc/motd':
  # #   content => "Welcome to your Vagrant-built virtual machine!
  # #               Managed by Puppet.\n"
  # # }
  #
  # config.vm.provision :puppet do |puppet|
  #   puppet.manifests_path = "manifests"
  #   puppet.manifest_file  = "base.pp"
  # end

  # Enable provisioning with chef solo, specifying a cookbooks path (relative
  # to this Vagrantfile), and adding some recipes and/or roles.
  #
  # config.vm.provision :chef_solo do |chef|
  #   chef.cookbooks_path = "cookbooks"
  #   chef.add_recipe "mysql"
  #   chef.add_role "web"
  #
  #   # You may also specify custom JSON attributes:
  #   chef.json = { :mysql_password => "foo" }
  # end

  config.vm.provision :chef_solo do |chef|
    chef.cookbooks_path = "cookbooks"
    chef.data_bags_path = "data_bags"
    chef.log_level = :debug

    chef.add_recipe "apt"
    chef.add_recipe "java"
    chef.add_recipe "ant"
    chef.add_recipe "git"
    chef.add_recipe "rng-tools"
    chef.add_recipe "apache2"
    chef.add_recipe "apache2::mod_dir"
    chef.add_recipe "apache2::mod_alias"
    chef.add_recipe "apache2::mod_ssl"
    chef.add_recipe "coldfusion10"
    chef.add_recipe "coldfusion10::apache"
    chef.add_recipe "coldfusion10::trustedcerts"
    chef.add_recipe "coldfusion10::configure"
    chef.add_recipe "railo334"
    chef.add_recipe "mxunit"
    chef.add_recipe "mysql::server"
    chef.add_recipe "coldfire-dev"

    chef.json = {
      "cf10" => {
        "installer" => {
          "file" => "ColdFusion_10_WWEJ_linux32.bin",
          "install_samples" => true
        },
        "config_settings" => {
          "runtime" => {
            "cacheProperty" => [ 
              {"propertyName" => "TrustedCache", "propertyValue" => false},
              {"propertyName" => "InRequestTemplateCache", "propertyValue" => false},
              {"propertyName" => "ComponentCache", "propertyValue" => false},
              {"propertyName" => "CacheRealPath", "propertyValue" => false},
              {"propertyName" => "SaveClassFiles", "propertyValue" => false}
            ]
          }
        }
      },
      ## Put your ColdFire repo settings here. ##
      #  "coldire-dev" => {
      #    "git" => {
      #      "repository" => "",
      #      "revision" => "",
      #      "deployment_key" => ""
      #    }
      #  },
      ## # # # # # # # # # # # # # # # # # # # ##
      "java" => {
        "install_flavor" => "oracle",
        "java_home" => "/usr/lib/jvm/java-6-oracle",
        "oracle" => {
          "accept_oracle_download_terms" => true
        }        
      },
      "mysql" => {
        "server_debian_password" => "vagrant",
        "server_root_password" => "vagrant",
        "server_repl_password" => "vagrant",
        "bind_address" => "0.0.0.0",
        "allow_remote_root" => true 
      }
    }
  end

  # Enable provisioning with chef server, specifying the chef server URL,
  # and the path to the validation key (relative to this Vagrantfile).
  #
  # The Opscode Platform uses HTTPS. Substitute your organization for
  # ORGNAME in the URL and validation key.
  #
  # If you have your own Chef Server, use the appropriate URL, which may be
  # HTTP instead of HTTPS depending on your configuration. Also change the
  # validation key to validation.pem.
  #
  # config.vm.provision :chef_client do |chef|
  #   chef.chef_server_url = "https://api.opscode.com/organizations/ORGNAME"
  #   chef.validation_key_path = "ORGNAME-validator.pem"
  # end
  #
  # If you're using the Opscode platform, your validator client is
  # ORGNAME-validator, replacing ORGNAME with your organization name.
  #
  # IF you have your own Chef Server, the default validation client name is
  # chef-validator, unless you changed the configuration.
  #
  #   chef.validation_client_name = "ORGNAME-validator"
end
