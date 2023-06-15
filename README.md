## Install Red Hat Single Sign-On 7.6 - Standalone - RHEL 8.8

### Step 01) Install openjdk and openjdk-devel 
Install OpenJDK
```sh
dnf install java-11-openjdk java-11-openjdk-devel -y
```

### Step 02) Download the Red Hat Single Sign-On 7.6 server and Red Hat Single Sign-On 7.6.3 server patch
```sh
#.../files/
cd /opt
rh-sso-7.6.0-server-dist.zip
rh-sso-7.6.3-patch.zip
```

### Step 03) Installing RH-SSO from a ZIP file
```sh
cd /opt
unzip rh-sso-7.6.0-server-dist.zip
cd rh-sso-7.6/bin
```

### Step 03) Apply the patch
```sh
cd /opt/rh-sso-7.6/bin
sh jboss-cli.sh
```
```sh
patch apply /opt/rh-sso-7.6.3-patch.zip
```
```sh
quit
```

### Step 04) Create user
```sh
useradd jboss-eap
chown -R jboss-eap:jboss-eap /opt/rh-sso-7.6
```

### Step 05) Start service RH-SSO
```sh
cd /opt/rh-sso-7.6/bin/init.d/
```
```sh
vim jboss-eap.conf
```
```sh
## Location of JBoss EAP
JBOSS_HOME="/opt/rh-sso-7.6"

## The username who should own the process.
JBOSS_USER=jboss-eap

## The mode JBoss EAP should start, standalone or domain
JBOSS_MODE=standalone

## Configuration for standalone mode
JBOSS_CONFIG=standalone.xml

## Location to keep the console log
JBOSS_CONSOLE_LOG="/var/log/jboss-eap/console.log"

## Additionals args to include in startup
JBOSS_OPTS="-b 0.0.0.0"
```
Copy files:
```sh
cp jboss-eap.conf /etc/default/
cp jboss-eap-rhel.sh /etc/init.d/
chmod +x /etc/init.d/jboss-eap-rhel.sh
chkconfig --add jboss-eap-rhel.sh
```
Start service RH-SSO
```sh
systemctl start jboss-eap-rhel
systemctl enable jboss-eap-rhel
systemctl status jboss-eap-rhel
```

### Step 06) Rule firewall
```sh
firewall-cmd --permanent --zone=public --add-port=8080/tcp
firewall-cmd --permanent --zone=public --add-port=9990/tcp
firewall-cmd --reload
firewall-cmd --zone=public --permanent --list-ports
```

### Step 07) Validate service
```sh
Open your browser and go to http://localhost:8080/auth to try it out.
```

<img title="RH-SSO" alt="Alt text" src="files/rh-sso.JPG">

## By

[Rolly V.](https://www.linkedin.com/in/rolly-s-villegas-delgado-aa9b9563/)
