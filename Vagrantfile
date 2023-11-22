Vagrant.configure("2") do |config|
  # Configuración del router
  config.vm.define :router do |router|
    router.vm.box = "debian/bullseye64"
    router.vm.hostname = "router"
    router.vm.synced_folder ".", "/vagrant", disabled: true

    router.vm.network :public_network,
      :dev => "br0",
      :mode => "bridge",
      :type => "bridge",
      use_dhcp_assigned_default_route: true

    router.vm.network :private_network,
      :libvirt__network_name => "red_intra",
      :libvirt__dhcp_enabled => false,
      :ip => "192.168.1.1",
      :libvirt__forward_mode => "veryisolated"
  end

  config.vm.define :web do |web|
    web.vm.box = "debian/bullseye64"
    web.vm.hostname = "web"
    web.vm.synced_folder ".", "/vagrant", disabled: true

    web.vm.network :private_network,
      :libvirt__network_name => "red_intra",
      :libvirt__dhcp_enabled => false,
      :ip => "192.168.1.2",
      :libvirt__forward_mode => "veryisolated"

    web.vm.network :private_network,
      :libvirt__network_name => "red_datos",
      :libvirt__dhcp_enabled => false,
      :ip => "10.0.0.2", 
      :libvirt__forward_mode => "veryisolated"
  end

  config.vm.define :bd do |bd|
    bd.vm.box = "debian/bullseye64"
    bd.vm.hostname = "bd"
    bd.vm.synced_folder ".", "/vagrant", disabled: true

    bd.vm.network :private_network,
      :libvirt__network_name => "red_intra",
      :libvirt__dhcp_enabled => false,
      :ip => "192.168.1.3", 
      :libvirt__forward_mode => "veryisolated"

    bd.vm.network :private_network,
      :libvirt__network_name => "red_datos",
      :libvirt__dhcp_enabled => false,
      :ip => "10.0.0.3", 
      :libvirt__forward_mode => "veryisolated"
  end
end
