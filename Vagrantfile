Vagrant.configure("2") do |config|
  # Configuración común para todas las máquinas
  config.vm.box = "ubuntu/bionic64"
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "512"
    vb.cpus = 1
  end

  # Máquina 1 - Servidor Apache 1
  config.vm.define "apache1" do |apache1|
    apache1.vm.network "private_network", type: "static", ip: "192.168.56.10"
    apache1.vm.provider "virtualbox" do |vb|
      vb.name = "apache1"
    end
    apache1.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update
      sudo apt-get install -y apache2
      sudo systemctl enable apache2
      sudo systemctl start apache2
    SHELL
  end

  # Máquina 2 - Servidor Apache 2
  config.vm.define "apache2" do |apache2|
    apache2.vm.network "private_network", type: "static", ip: "192.168.56.11"
    apache2.vm.provider "virtualbox" do |vb|
      vb.name = "apache2"
    end
    apache2.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update
      sudo apt-get install -y apache2
      sudo systemctl enable apache2
      sudo systemctl start apache2
    SHELL
  end

  # Máquina 3 - Servidor Nginx (Balanceador de carga)
  config.vm.define "nginx" do |nginx|
    nginx.vm.network "private_network", type: "static", ip: "192.168.56.20"
    nginx.vm.provider "virtualbox" do |vb|
      vb.name = "nginx"
    end
    nginx.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update
      sudo apt-get install -y nginx
      sudo systemctl enable nginx
      sudo systemctl start nginx
      # Configuración de Load Balancer en Nginx
      sudo bash -c 'echo "upstream backend {
        server 192.168.56.10;
        server 192.168.56.11;
      }
      server {
        listen 80;
        location / {
          proxy_pass http://backend;
        }
      }" > /etc/nginx/sites-available/default'
      sudo systemctl restart nginx
    SHELL
  end
end
