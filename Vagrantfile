Vagrant.require_version ">= 2.2.5"

ENV["LC_ALL"] = "it_IT.UTF-8"

#yum -y install net-tools telnet
#yum -y remove docker docker-client docker-client-latest docker-common docker-latest docker-latest-logrotate docker-logrotate docker-engine
#yum -y install -y yum-utils device-mapper-persistent-data lvm2
#yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
#yum -y install docker-ce-18.09.9 docker-ce-cli-18.09.9 containerd.io
#mkdir -p /etc/docker
#mv /tmp/daemon.json /etc/docker/daemon.json
#mkdir -p /etc/systemd/system/docker.service.d
#systemctl enable docker
#systemctl start docker
#mv /tmp/kubernetes.repo /etc/yum.repos.d/kubernetes.repo
#setenforce 0
#sed -i 's/^SELINUX=enforcing$/SELINUX=permissive/' /etc/selinux/config
#mv /tmp/k8s.conf /etc/sysctl.d/k8s.conf
#sysctl --system
#sed -e '/swap/s/^/#/g' -i /etc/fstab
#yum -y install kubelet kubeadm kubectl --disableexcludes=kubernetes
#kubectl completion bash > /etc/bash_completion.d/kubectl
#systemctl enable kubelet

#firewall-cmd --permanent --add-port={6443,2379,2380,10250,10251,10252}/tcp
#firewall-cmd --reload
#kubeadm init --apiserver-advertise-address=192.168.150.2 --pod-network-cidr=10.244.0.0/16
#su - vagrant -c "mkdir -p /home/vagrant/.kube"
#su - vagrant -c "cp -i /etc/kubernetes/admin.conf /home/vagrant/.kube/config"
#su - vagrant -c "kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/62e44c867a2846fefb68bd5f178daf4da3095ccb/Documentation/kube-flannel.yml"
#su - vagrant -c "kubectl taint nodes --all node-role.kubernetes.io/master-"
#su - vagrant -c "kubeadm token create --print-join-command > /vagrant/join.sh"
#su - vagrant -c "kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0-beta4/aio/deploy/recommended.yaml"
#su - vagrant -c "kubectl apply -f /tmp/kubernetes-dashboard-public.yml"

#$master_script = <<-EOF
#EOF

#firewall-cmd --permanent --add-port={10250,30000-32767}/tcp
#firewall-cmd --reload

$worker_script = <<-EOF
. /vagrant/join.sh
EOF

Vagrant.configure("2") do |config|
  config.vm.provider 'virtualbox' do |v|
    v.memory = 2048
    v.cpus = 2
  end

  config.vm.synced_folder ".", "/vagrant", type: "virtualbox", automount: true

  config.vm.define "master" do |node|
    node.vm.box = "centos/7"
    node.vm.hostname = "master"
    node.vm.network 'private_network', ip: '192.168.150.2'
    node.vm.provision "file", source: "kubernetes.repo", destination: "/tmp/kubernetes.repo"
    node.vm.provision "file", source: "k8s.conf", destination: "/tmp/k8s.conf"
    node.vm.provision "file", source: "daemon.json", destination: "/tmp/daemon.json"
    node.vm.provision "file", source: "kubernetes-dashboard-public.yml", destination: "/tmp/kubernetes-dashboard-public.yml"
    node.vm.provision "ansible", playbook: "playbook.yml"
    #node.vm.provision "shell", inline: $master_script
  end

  config.vm.define "worker" do |node|
    node.vm.box = "centos/7"
    node.vm.hostname = "worker"
    node.vm.network 'private_network', ip: '192.168.150.3'
    node.vm.provision "file", source: "kubernetes.repo", destination: "/tmp/kubernetes.repo"
    node.vm.provision "file", source: "k8s.conf", destination: "/tmp/k8s.conf"
    node.vm.provision "file", source: "daemon.json", destination: "/tmp/daemon.json"
    #node.vm.provision "ansible", playbook: "worker-playbook.yml"
    #node.vm.provision "shell", inline: $worker_script
  end
end
