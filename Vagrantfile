VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "ubuntu/trusty64"
  config.vm.network "forwarded_port", guest: 4444, host: 4444 #hub
  config.vm.network "forwarded_port", guest: 5555, host: 5555 #node
  config.vm.network "forwarded_port", guest: 5999, host: 5999 #vnc


  $script_selenium = <<SCRIPT

echo ==== Create a selenium folder ====
mkdir /usr/local/selenium


echo ==== Installing dependencies, curl, wget, unzip ====
apt-get update -y
apt-get install wget curl unzip -y



echo ==== Installing Java ====
#ORACLE JAVA
#sudo add-apt-repository ppa:webupd8team/java
#sudo apt-get update
#sudo apt-get install oracle-java7-installer

#OPEN JAVA
apt-get install openjdk-7-jre -y
#apt-get install openjdk-7-jdk -y
#apt-get install ant -y


echo ==== Installing firefox ====
wget http://ftp.mozilla.org/pub/mozilla.org/firefox/releases/33.0.2/linux-x86_64/en-US/firefox-33.0.2.tar.bz2  && \
tar xjf firefox-33.0.2.tar.bz2 && \
mv firefox /opt/firefox-33.0.2 && \
ln -s /opt/firefox-33.0.2/firefox /usr/bin/firefox

echo ==== Installing chrome ====
#wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | sudo apt-key add -
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
mv *.deb /usr/local/selenium
dpkg -i /usr/local/selenium/google-chrome*; sudo apt-get -f install -y


echo ==== Installing chromedriver ====
wget http://chromedriver.storage.googleapis.com/2.10/chromedriver_linux64.zip
mv *.zip /usr/local/selenium
unzip /usr/local/selenium/*.zip -d /usr/local/selenium


echo ==== Setting up x11vnc ====
apt-get install x11vnc -y -q
RUN apt-get install -y -q xfonts-100dpi xfonts-75dpi xfonts-scalable xfonts-cyrillic

echo ==== Setting up Xvfb ====
apt-get install xvfb -y -q

# INIT for Xvfb (turn to upstart)
cp /vagrant/Xvfb /etc/init.d/.
update-rc.d Xvfb defaults
service Xvfb start

echo ==== Setting up selenium ====
wget http://selenium-release.storage.googleapis.com/2.43/selenium-server-standalone-2.43.1.jar
mv *.jar /usr/local/selenium/.

#Run two jars, one for grid, one for hub. Why? let's try to register several versions of firefox
#INIT for selenium-hub
cp /vagrant/selenium-grid /etc/init.d/.
update-rc.d selenium-grid defaults
service selenium-grid start

#INIT for selenium-node
cp /vagrant/selenium-node /etc/init.d/.
update-rc.d selenium-node defaults
service selenium-node start


#echo ==== Starting vnc server ==== (make upstart from it)
#1024x768x16
#/usr/bin/x11vnc -display :99 -N -forever >/dev/null &2>1 &


SCRIPT
  config.vm.provision :shell, :inline => $script_selenium
end
