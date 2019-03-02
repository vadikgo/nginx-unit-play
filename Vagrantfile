Vagrant.configure(2) do |config|

  config.vm.box = "ubuntu/bionic64"

  config.vm.provider "virtualbox" do |vb|
    vb.gui = false
    vb.memory = "1024"
    vb.gui = false
    vb.linked_clone = true
  end

  config.vm.hostname = "unit"
  config.vm.network :forwarded_port, guest: 8800, host: 8800
  config.vm.network :forwarded_port, guest: 8080, host: 8080

  config.vm.provision "shell", inline: <<-SHELL
    sudo apt update
    sudo apt install gnupg2 curl -y
    curl -s https://nginx.org/keys/nginx_signing.key | sudo apt-key add
    grep 'nginx.org.unit' /etc/apt/sources.list || echo 'deb https://packages.nginx.org/unit/ubuntu/ bionic unit' | sudo tee -a /etc/apt/sources.list
    sudo apt update
    sudo apt install unit-go1.10 unit-dev unit-jsc-common unit-jsc8 -y
    sudo systemctl enable unit.service
    sudo systemctl start unit.service
    cd /usr/share/doc/unit-jsc8/examples
    sudo curl -X PUT --data-binary @unit.config --unix-socket /var/run/control.unit.sock http://localhost/config
  SHELL
end
