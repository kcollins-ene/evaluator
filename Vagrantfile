# -*- mode: ruby -*-
# vi: set ft=ruby :

$install = <<SCRIPT
# Initialize install log
rm -f install.log
# Configure for noninteractive mode (for dpkg)
export DEBIAN_FRONTEND=noninteractive
# Prevent accessing stdin when no terminal available in root profile
sudo sed -i 's/^mesg n/tty -s \\&\\& mesg n/g' /root/.profile
sudo ex +"%s@DPkg@//DPkg" -cwq /etc/apt/apt.conf.d/70debconf
sudo dpkg-reconfigure debconf -f noninteractive -p critical
# Add Oracle Java Repository
echo "Adding Oracle Java Repository"
sudo add-apt-repository -y ppa:webupd8team/java >> install.log 2>&1
sudo apt-get update >> install.log
# Setup License Acceptance and install Java8
echo "Installing Oracle Java8"
echo debconf shared/accepted-oracle-license-v1-1 select true | sudo debconf-set-selections
echo debconf shared/accepted-oracle-license-v1-1 seen true | sudo debconf-set-selections
sudo apt-get install -y -q oracle-java8-installer >> install.log
# Setup MySQL Setup and install mysql-server
echo "Installing MySQL"
echo "mysql-server mysql-server/root_password select ignitionsql" | sudo debconf-set-selections
echo "mysql-server mysql-server/root_password_again select ignitionsql" | sudo debconf-set-selections
sudo apt-get install -y -q mysql-server >> install.log
# Setup MySQL Username
echo "Setting up 'ignition' database with 'ignition' user and password 'ignition'"
mysql -u root --password=ignitionsql -e "CREATE USER 'ignition'@'localhost' IDENTIFIED BY 'ignition'; CREATE DATABASE ignition; GRANT ALL PRIVILEGES ON ignition.* to 'ignition'@'localhost';" >> install.log
# Download Ignition and install
echo "Downloading Ignition 7.9.10"
wget -q --referer https://inductiveautomation.com/* https://s3.amazonaws.com/files.inductiveautomation.com/release/ia/build7.9.10/20181128-1303/zip-installers/Ignition-7.9.10-linux-x64-installer.run -O /vagrant/ignition-installer.run >> install.log
chmod a+x /vagrant/ignition-installer.run
echo "Installing Ignition 7.9.10"
sudo /vagrant/ignition-installer.run --unattendedmodeui none --mode unattended --prefix /usr/local/share/ignition >> install.log
echo "Starting Ignition"
sudo systemctl start ignition.service
SCRIPT

# Vagrant Configuration
Vagrant.configure("2") do |config|
  # Use an Ubuntu Xenial (16.04 LTS) template
  config.vm.box = "ubuntu/xenial64"

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8088" will access port 8088 on the guest machine.
  config.vm.network "forwarded_port", guest: 8088, host: 8088, host_ip: "127.0.0.1", auto_correct: true

  # System Configuration
  config.vm.provider "virtualbox" do |vb|
    vb.linked_clone = true
    vb.memory = "2048"
  end

  config.vm.provider "parallels" do |prl, override|
    override.vm.box = "parallels/ubuntu-16.04"
    prl.linked_clone = true
    prl.memory = 2048
  end

  # Provisioning
  config.ssh.shell = "bash -c 'BASH_ENV=/etc/profile exec bash'"
  config.vm.provision "shell", inline: $install
end
