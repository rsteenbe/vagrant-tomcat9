Vagrant.configure("2") do |config|
  config.vm.provider "virtualbox" do |v|
	v.memory = 4096
  end
  config.vm.box = "ubuntu/xenial64"
  config.vm.network "private_network", ip: "192.168.50.12"
  
  # Port 
  config.vm.network 'forwarded_port', guest: 80, host: 80
  config.vm.network 'forwarded_port', guest: 1099, host: 1099
  config.vm.network 'forwarded_port', guest: 1100, host: 1100
  config.vm.network 'forwarded_port', guest: 8080, host: 8080
  
  config.vm.provision 'shell', inline: <<-SHELL
	# Box Configuration
	export TOMCAT_VERSION=9.0.14
  
	# Update machine
	apt-get update

	# Download & Install Java
	apt-get install openjdk-8-jdk -y

	# Download, Extract & Change Permissions
	groupadd tomcat
	cd /tmp
	wget -q http://apache.mirror.ipcheck.nu/tomcat/tomcat-9/v${TOMCAT_VERSION}/bin/apache-tomcat-${TOMCAT_VERSION}.tar.gz
	mkdir /opt/tomcat
	tar xzvf /tmp/apache-tomcat-${TOMCAT_VERSION}.tar.gz -C /opt/tomcat --strip-components=1
	cd /opt/tomcat
	chmod -R 777 /opt/tomcat
	cp /vagrant/tomcat.service /etc/systemd/system/tomcat.service
	
	# Add User & Change File Locking Configuration
	sed -i 's|</tomcat-users>|<user username="admin" password="password" roles="manager-gui,admin-gui"/></tomcat-users>|' /opt/tomcat/conf/tomcat-users.xml
	sed -i 's|<Context antiResourceLocking="false" privileged="true" >|<Context antiResourceLocking="false" privileged="true" ><!--|' /opt/tomcat/webapps/manager/META-INF/context.xml
	sed -i 's|</Context>|--></Context>|' /opt/tomcat/webapps/manager/META-INF/context.xml
	sed -i 's|<Context antiResourceLocking="false" privileged="true" >|<Context antiResourceLocking="false" privileged="true" ><!--|' /opt/tomcat/webapps/host-manager/META-INF/context.xml
	sed -i 's|</Context>|--></Context>|' /opt/tomcat/webapps/host-manager/META-INF/context.xml
	
	# Start Tomcat
	systemctl daemon-reload
	systemctl enable tomcat
	systemctl restart tomcat
	
	# Print Info
	echo ''
	echo '========== VAGRANT TOMCAT 9 BOX =========='
	echo ''
	echo 'Tomcat URL: http://localhost:8080/'
	echo 'User: admin'
	echo 'Password: password'
	echo ''
	echo 'SSH: vagrant ssh'
	echo 'Box ip-address: 192.168.50.12'
	echo ''
	echo '=========================================='
	echo ''
  SHELL
end