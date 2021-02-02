# Vagrantfile to create an Ubuntu VM on Platform9 OpenStack
# In our environment we have to boot from a new volume rather
# than booting directly from an image, hence the `volume_boot`
# config hash.

$script = <<SCRIPT
pwd
mkdir downloads
SCRIPT

$script1 = <<SCRIPT
pwd
mkdir downloads
SCRIPT


Vagrant.configure("2") do |config|
  config.ssh.private_key_path = ENV['OS_SSH_KEY_PATH']
  config.ssh.insert_key = true
  config.vm.provider :openstack do |os|
    os.openstack_auth_url   = ENV['OS_AUTH_URL']
    os.identity_api_version = ENV['OS_IDENTITY_API_VERSION']
    os.username             = ENV['OS_USERNAME']
    os.password             = ENV['OS_PASSWORD']
    os.keypair_name         = ENV['OS_KEYPAIR_NAME']
    os.project_name         = ENV['OS_PROJECT_NAME']
    os.region               = ENV['OS_REGION_NAME']
    os.domain_name          = ENV['OS_PROJECT_DOMAIN_ID']
    os.floating_ip_pool     = 'external'
    os.networks             = ['network1']
    os.security_groups      = ['default']
  end

  # KILL SMB WITH FIRE
  config.vm.allowed_synced_folder_types = [:rsync]

  #config.vm.box       = 'openstack'
  config.vm.guest = :windows
  config.vm.communicator = 'winrm'
  config.winrm.guest_port = '5985'
  config.winrm.username = 'admin'
  config.winrm.password = 'puppetlabs'
  #vagrant-openstack provider can't live without below line
  config.ssh.username = 'admin'
  config.ssh.port = '5985'

  config.vm.provider :openstack do |os|
      os.server_name = 'Win-Agent'
	    os.flavor = 'm1.large'
      os.volume_boot = {
      size: 120,
      delete_on_destroy: true,
      image: 'win-2016-x86_64',
    }
    end
    #s.vm.provision :shell, inline: $script1
  end