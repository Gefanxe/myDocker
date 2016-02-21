Vagrant.configure(2) do |config|
  # 選擇基底box
  # boxes at https://atlas.hashicorp.com/search.
  config.vm.box = "ubuntu/trusty64"

  # 檢查box是否有更新
  # config.vm.box_check_update = false

  # 開port對應
  # config.vm.network "forwarded_port", guest: 80, host: 8080
  # 迴圈開port對應
  # for i in 3000..3010
  #   config.vm.network "forwarded_port", guest: i, host: i
  # end

  # Host-only Adapter
  # config.vm.network "private_network", type: "dhcp"
  config.vm.network "private_network", ip: "192.168.56.111"

  # 橋接公用網路
  # config.vm.network "public_network", use_dhcp_assigned_default_route: true

  #共用資料夾
  # config.vm.synced_folder "C:/Developer", "/Developer"

  # 定義虛擬機
  config.vm.provider "virtualbox" do |vb|
    vb.name = "docker"
    opts1 = ["modifyvm" , :id, "--groups", "/test", "--memory", "2048", "--paravirtprovider", "default", "--cpus", "2"]
    vb.customize opts1
  end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # 執行shell命令

  config.vm.provision "fix-no-tty", type: "shell" do |s|
    s.privileged = false
    s.inline = "sudo sed -i '/tty/!s/mesg n/tty -s \\&\\& mesg n/' /root/.profile"
  end
  
  config.vm.provision "file", source: "grub", destination: "grub"
  config.vm.provision "file", source: "docker", destination: "docker"

  config.vm.provision "shell", inline: <<-SHELL
    sudo apt-get update
    sudo apt-get install linux-image-extra-$(uname -r)
    sudo curl -sSL https://get.docker.com/ | sh
    sudo usermod -aG docker vagrant
    sudo cp ./grub /etc/default/grub
    sudo update-grub
    sudo cp ./docker /etc/default/docker
    sudo restart docker
    rm ./grub
    rm ./docker
  SHELL
end
