Vagrant.configure("2") do |config|
  config.vm.define "vnode-barge"

  config.vm.box = "ailispaw/barge"

  config.vm.network :private_network, ip: "192.168.33.10"

  config.vm.synced_folder ".", "/vagrant", type: "nfs",
    mount_options: ["nolock", "vers=3", "udp", "noatime", "actimeo=1"]

  config.vm.provision :shell, run: "always" do |sh|
    sh.inline = <<-EOT
      pkg install git

      if [ ! -f /opt/bin/docker-compose ]; then
        wget -q -O /opt/bin/docker-compose \
          https://github.com/docker/compose/releases/download/1.11.2/docker-compose-Linux-x86_64
        chmod +x /opt/bin/docker-compose
      fi
    EOT
  end

  config.vm.provision :shell do |sh|
    sh.privileged = false
    sh.inline = <<-EOT
      git config --global http.sslCAinfo /etc/ssl/certs/ca-certificates.crt

      cd /vagrant/vvm_node
      if [ ! -d vvm_notebook/docker-stacks ]; then
        VVM_USER=vivuser bash generate_vvmnb.sh
      fi
      VVM_USER=vivuser bash build.sh
      VVM_USER=vivuser bash up.sh
    EOT
  end
end
