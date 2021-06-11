# -*- mode: ruby -*-
# vi: set ft=ruby :

$lab_tdagentbit = <<-'SCRIPT'
timedatectl set-timezone Pacific/Auckland
echo "192.168.1.71 splunk.local splunk" >> /etc/hosts
yum install -y yum-utils
cat <<- 'EOF' > /etc/yum.repos.d/td-agent-bit.repo 
	[td-agent-bit]
	name = TD Agent Bit
	baseurl = https://packages.fluentbit.io/centos/7/$basearch/
	gpgcheck=1
	gpgkey=	https://packages.fluentbit.io/fluentbit.key
	enabled=1
EOF
yum install -y td-agent-bit
yum update -y
sudo service td-agent-bit start
SCRIPT

# tdagentbit VM
Vagrant.configure("2") do |config|
  config.vm.define "tdagentbit" do |tdagentbit|
    tdagentbit.vm.box = "centos/7"
    tdagentbit.vm.hostname = 'tdagent'
    tdagentbit.vm.network :private_network, ip: "172.16.94.50"
    tdagentbit.vm.provider :virtualbox do |v|
    tdagentbit.vm.synced_folder ".", "/vagrant", type: "virtualbox"
      v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      v.customize ["modifyvm", :id, "--memory", 1024]
      v.customize ["modifyvm", :id, "--name", "tdagentbit"]
    end
    tdagentbit.vm.provision "shell", inline: $lab_tdagentbit
  end
end
