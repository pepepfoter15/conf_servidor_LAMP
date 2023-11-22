Vagrant.configure("2") do |config|
  # Configuración del router
  config.vm.define :router do |router|
    router.vm.box = "debian/bullseye64"
    router.vm.hostname = "router"
    router.vm.synced_folder ".", "/vagrant", disabled: true

    # Red pública (br0)
    router.vm.network :public_network,
      :dev => "br0",
      :mode => "bridge",
      :type => "bridge",
      use_dhcp_assigned_default_route: true

    # Red privada muy aislada (red_intra)
    router.vm.network :private_network,
      :libvirt__network_name => "red_intra",
      :libvirt__dhcp_enabled => false,
      :ip => "192.168.1.1",
      :libvirt__forward_mode => "veryisolated"
  end

  # Configuración del servidor web (web)
  config.vm.define :web do |web|
    web.vm.box = "debian/bullseye64"
    web.vm.hostname = "web"
    web.vm.synced_folder ".", "/vagrant", disabled: true

    # Conexión al router por la red_intra
    web.vm.network :private_network,
      :libvirt__network_name => "red_intra",
      :libvirt__dhcp_enabled => false,
      :ip => "192.168.1.2",
      :libvirt__forward_mode => "veryisolated"

    # Conexión a la máquina bd por la red_datos (muy aislada)
    web.vm.network :private_network,
      :libvirt__network_name => "red_datos",
      :libvirt__dhcp_enabled => false,
      :ip => "10.0.0.2", 
      :libvirt__forward_mode => "veryisolated"
  end

  # Configuración del servidor de base de datos (bd)
  config.vm.define :bd do |bd|
    bd.vm.box = "debian/bullseye64"
    bd.vm.hostname = "bd"
    bd.vm.synced_folder ".", "/vagrant", disabled: true

    # Conexión al router por la red_intra
    bd.vm.network :private_network,
      :libvirt__network_name => "red_intra",
      :libvirt__dhcp_enabled => false,
      :ip => "192.168.1.3", 
      :libvirt__forward_mode => "veryisolated"

    # Conexión a la máquina web por la red_datos (muy aislada)
    bd.vm.network :private_network,
      :libvirt__network_name => "red_datos",
      :libvirt__dhcp_enabled => false,
      :ip => "10.0.0.3", 
      :libvirt__forward_mode => "veryisolated"
  end
end
