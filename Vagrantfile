s# -*- mode: ruby -*-
# vi: set ft=ruby :

# This Vagrantfile is intended to be used in lab environment only.
#
# Author: Antonio Alisio de Meneses Cordeiro
# email: alisio.meneses@gmail.com

Vagrant.configure("2") do |config|
  config.vm.box = "centos/7"
  config.vm.network "forwarded_port", guest: 5060, host: 5060, protocol: "udp"
  config.vm.network "forwarded_port", guest: 4569, host: 4569, protocol: "udp"
  config.vm.network "forwarded_port", guest: 5038, host: 5038, protocol: "tcp"
  for i in 10000..10006
    config.vm.network :forwarded_port, guest: i, host: i, protocol: "udp"
  end
  config.vm.synced_folder ".", "/vagrant", type:"virtualbox"
  config.vm.provision "shell", inline: <<-SHELL
    ########
    vboxVersion=5.2.22
    asteriskVersion=11
    releasever=7
    basearch=x86_64
    ########
    [[ -f provisioned.ok ]] && echo Already provisioned && exit
    cd /tmp/
    yum install -y --nogpgcheck dnf epel-release kernel-devel kernel-headers vim wget
    yum groupinstall -y "Development Tools"
    wget https://download.virtualbox.org/virtualbox/${vboxVersion}/VBoxGuestAdditions_${vboxVersion}.iso
    mount -o loop -t iso9660 VBoxGuestAdditions_${vboxVersion}.iso /mnt/
    /mnt/VBoxLinuxAdditions.run
    sed -i "s/SELINUX=enforcing/SELINUX=disabled/g" /etc/selinux/config
    # asterisk install
    cat << EOF > /etc/yum.repos.d/tucny-asterisk${asteriskVersion}.repo
[asterisk-common]
name=Asterisk Common Requirement Packages @ tucny.com
#baseurl=https://ast.tucny.com/repo/asterisk-common/el$releasever/$basearch/
mirrorlist=https://ast.tucny.com/mirrorlist.php?release=$releasever&arch=$basearch&repo=asterisk-common
enabled=1
gpgcheck=0

[asterisk-${asteriskVersion}]
name=Asterisk ${asteriskVersion} Packages @ tucny.com
mirrorlist=https://ast.tucny.com/mirrorlist.php?release=$releasever&arch=$basearch&repo=asterisk-${asteriskVersion}
enabled=1
gpgcheck=0
EOF
  cat << EOF > /etc/yum.repos.d/irontec.repo
[irontec]
name=Irontec RPMs repository
baseurl=http://packages.irontec.com/centos/$releasever/$basearch/
gpgcheck=0
EOF
    yum install -y asterisk asterisk-oss ngrep sngrep
    mv /etc/asterisk /etc/asterisk.original
    ln -s /vagrant/assets/asterisk /etc/asterisk
    touch /var/log/asterisk/full
    chown asterisk:asterisk /var/log/asterisk/full
    systemctl enable asterisk
    systemctl stop asterisk
    systemctl start asterisk
    touch provisioned.ok
  SHELL
end
