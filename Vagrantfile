ENV["LC_ALL"] = "en_US.UTF-8"

$script = <<-EOF
export LC_ALL=en_US.UTF-8
sudo yum -y update
sudo yum -y install wget
sudo cp /vagrant/jenkins/nginx.repo /etc/yum.repos.d/nginx.repo
sudo wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat-stable/jenkins.repo
sudo wget -O /etc/yum.repos.d/nginx.repo https://raw.githubusercontent.com/SBohomolov/vagrant-jenkins/master/nginx.repo
sudo rpm --import https://jenkins-ci.org/redhat/jenkins-ci.org.key
sudo yum -y install jenkins nginx net-tools policycoreutils-python java mc htop vim
sudo wget -O /etc/sysconfig/jenkins https://raw.githubusercontent.com/SBohomolov/vagrant-jenkins/master/jenkins
sudo wget -O /etc/nginx/conf.d/default.conf https://raw.githubusercontent.com/SBohomolov/vagrant-jenkins/master/default.conf
sudo semanage port -a -t http_port_t -p tcp 2222
sudo setsebool -P httpd_can_network_connect 1
sudo service jenkins start
sudo service nginx start
sudo systemctl enable nginx
sudo systemctl enable jenkins
EOF

Vagrant.configure("2") do |config|
  config.vm.define "test-vagrant" do |test|
    test.vm.box = "centos/7"
    test.vm.hostname = "stan.box"
    test.vm.network "private_network", ip: "192.168.25.17"
    test.vm.network "public_network", bridge: 'wlp2s0', use_dhcp_assigned_default_route: true

    test.vm.provision "shell", inline: $script
  end
end

