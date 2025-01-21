Vagrant.configure("2") do |config|
  # Master Node
  config.vm.define "master" do |master|
    master.vm.box = "alvistack/ubuntu-22.04"
    master.vm.hostname = "master"
    master.vm.network "private_network", ip: "10.10.100.20"
    master.vm.provider "virtualbox" do |vb|
      vb.memory = "4096"
      vb.cpus = 2
    end
    master.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get install -y docker.io
      systemctl start docker
      systemctl enable docker
      apt-get install -y apt-transport-https curl

# This script has some issues when pulling the keyring to the machine and verifying
      curl -fsSL https://pkgs.k8s.io/core:/stable:/1.32/deb/Release.key |
        gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
      echo "deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/1.32/deb/ /" |
    tee /etc/apt/sources.list.d/kubernetes.list
# When the script above doesn't run properly, then the ones below will also cause "not found" therefore not installed.
      apt-get update
      apt-get install -y kubelet kubeadm kubectl
      apt-mark hold kubelet kubeadm kubectl
    SHELL
  end
 
  # Worker Nodes
  (1..2).each do |i|
    config.vm.define "worker#{i}" do |worker|
      worker.vm.box = "alvistack/ubuntu-22.04"
      worker.vm.hostname = "worker#{i}"
      worker.vm.network "private_network", ip: "10.10.100.2#{i}"
      worker.vm.provider "virtualbox" do |vb|
        vb.memory = "2048"
        vb.cpus = 2
      end
      worker.vm.provision "shell", inline: <<-SHELL
        apt-get update
        apt-get install -y docker.io
        systemctl start docker
        systemctl enable docker
        apt-get install -y apt-transport-https curl

# Same as master
        curl -fsSL https://pkgs.k8s.io/core:/stable:/1.32/deb/Release.key |
          gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
        echo "deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/1.32/deb/ /" |
          tee /etc/apt/sources.list.d/kubernetes.list
        apt-get update
        apt-get install -y kubelet kubeadm kubectl
        apt-mark hold kubelet kubeadm kubectl
      SHELL
    end
  end
end
