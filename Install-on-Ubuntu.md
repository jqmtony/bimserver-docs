# Latest updates

The instructions below are outdated (for version 1.3; last update 2014).

Other users also wrote instructions about installation of BIMserver on several systems.
Feel free to add your link to this list:
* (June 2017) https://fumblehool.wordpress.com/2017/06/17/install-bim-server-on-ubuntu-server/
* (October 2015) https://bhushanbharat.wordpress.com/2015/10/13/install-bimserver-on-ubuntu-14-04/
* (July 2014) https://arshpreetsingh.wordpress.com/2014/07/20/install-bimserver-on-ubuntu/ 


***
***



# Outdated instructions

> These instructions are for the 1.3 release.
> Updated on 06-10-2014 while installing BIMserver 1.3 on a 122GB AWS machine
> This tutorial assumes "root" privileges, you might have to add "sudo" to all statements.
> This tutorial assumes you already have a working (AWS) server. Make sure you enable the right ports on it as well (80 or 8080)

How to install the BIMserver on a freshly installed Ubuntu 12.04.2 LTS Server Amazon EC2 server.

AMI: Ubuntu Cloud Guest AMI ID ami-d0f89fb9 (x86_64)
Instance: m1.xlarge machine

Of course most (if not all) of the instructions are generic enough for any Ubuntu installation.

This guide can be used to install a BIMserver on a cloud based server. Root rights are assumed. When running as a normal user, prepend "sudo " to each command (or start your session with sudo -i).

Paths can be changed to your liking of course. This is just one way of doing things, yes you can use a different NTP server, yes you can use another JRE implementation etc...

Directories chosen for this installation:
```
/var/www/[YOUR DOMAIN]/ROOT.war // For the BIMserver WAR file
/opt/tomcat7                    // For the Tomcat7 application
/var/bimserver/home             // For the BIMserver home directory
```

[YOUR DOMAIN] should be a DNS entry (can be a subdomain).

# Commands

## Basic configuration of Ubuntu
| Command | Description |
| ------------- | ------------- |
| apt-get update | Update the package index |
| apt-get upgrade | Upgrade installed packages |
| apt-get install openjdk-7-jre-headless wget unzip nano ntpdate | Install JRE 7 and some tools |
| ntpdate 0.nl.pool.ntp.org | Update the local time, useful when looking in log files |
| dpkg-reconfigure tzdata | Select timezone |

## Create directories / users
| Command | Description |
| ------------- | ------------- |
| mkdir /var/www | Create a /var/www directory if it's not already there |
| mkdir /var/www/[YOUR DOMAIN] | Create a directory for you domain |
| useradd -s /sbin/nologin tomcat7 | Create a tomcat7 user |
| chown -R tomcat7 /var/www/[YOUR DOMAIN] | Give rights to tomcat7 user to write |
| mkdir /var/bimserver | Create a directory for you BIMserver home directory (this has nothing to do with /home on unix systems |
| chown -R tomcat7 /var/bimserver | Give the appropriate rights to the tomcat7 user |

## Install Tomcat7
| Command | Description |
| ------------- | ------------- |
| cd /opt | Goto the /opt directory |
| wget [location of tomcat] -O tomcat7.zip | Download tomcat (Make sure you replace this with the latest release!) |
| unzip [filename] | Unzip Tomcat7 |
| rm [filename] | Remove the downloaded zip file |
| mv [extracted directory] tomcat7 | Rename to convenient name |
| chmod +x /opt/tomcat7/bin/*.sh | Make .sh files executable |
| mkdir /opt/tomcat7/conf/policy.d | Create a policy directory |
| nano /opt/tomcat7/conf/policy.d/default.policy | Edit the default policy file |

Paste the following default code (you can change this later!)
```
grant {
  permission java.security.AllPermission;
};
```

| Command | Description |
| ------------- | ------------- |
| chown -R tomcat7 /opt/tomcat7 | Change owner of directory tot tomcat7 |

Change the Tomcat7 configuration file:
```
nano /opt/tomcat7/conf/server.xml
```
Change the port attribute in the Connector tag to the desired port (also see: "Running op ports below 1024". Also add a new host, see below.

```
<Host name="[YOUR DOMAIN]" appBase="/var/www/[YOUR DOMAIN]" unpackWARs="true" autoDeploy="true" xmlValidation="false" xmlNamespaceAware="false">
    <Context path="" docBase="/var/www/[YOUR DOMAIN]/ROOT.war">
        <Parameter name="homedir" value="/var/bimserver/home"/>
    </Context>
</Host>
```

## Setting the homedir

By default, BIMserver will store the database folder, the log files etc... in the WEB-INF folder of the extracted .war file of BIMserver. We will call this folder the "home" folder. This directory will be removed when you upgrade your BIMserver to a new version because the WEB-INF folder is part of your web application folder. To tell BIMserver to store your "home" directory somewhere else, you can set a parameter in your application server configuration (in this case, Tomcat).

There are multiple ways, the easiest is to add it to your Host configuration in server.xml (you might already have done this while setting up the host)
```
<Host name="[YOUR DOMAIN]" appBase="/var/www/[YOUR DOMAIN]" unpackWARs="true" autoDeploy="true" xmlValidation="false" xmlNamespaceAware="false">
    <Context path="" docBase="/var/www/[YOUR DOMAIN]/ROOT.war">
        <Parameter name="homedir" value="/var/bimserver/home"/>
    </Context>
</Host>
```

Another way is to add it to a default context of a configured virtual host:
```
<Context path="" docBase="/var/www/[YOUR DOMAIN]/ROOT.war">
        <Parameter name="homedir" value="/var/bimserver/home"/>
</Context>
```

This default context file can be found/created here:
```
[TOMCAT INSTALLATION]/conf/Catalina/[VIRTUALHOST]/context.xml.default
```

There are other options as well, for those please see the [Tomcat website](http://tomcat.apache.org/tomcat-7.0-doc/config/context.html).

# Startup script

To be able to starts/stop/restart tomcat7 you need an init.d script. You can find one [https://gist.github.com/baylisscg/942150 here]. Copy this file to /etc/init.d/tomcat7 and give it execute permissions (`chmod +x /etc/init.d/tomcat7`).

Open the file for editing and find the following variable declarations and change them to:
```bash
CATALINA_HOME=/opt/$NAME
CATALINA_BASE=/opt/$NAME
TOMCAT7_SECURITY=no // You can change this to yes later on
JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64/jre // Change this to a JRE directory of your choosing, run 'update-alternatives --list java' to see the available ones. Make sure you point to a JRE directory, for example: /usr/lib/jvm/java-7-openjdk-amd64/jre
```

Find the line that says:
```bash
JAVA_OPTS="-Djava.awt.headless=true -Xmx4G"
```
and change the amount of heap memory to your liking. Always keep a few hundred megabytes free for your OS and other applications.

Restart Tomcat: `service tomcat7 restart`

## Install BIMserver
| Command | Description |
| ------------- | ------------- |
| cd /var/www/[YOUR DOMAIN] | Go to your domain folder |
| wget [location of war file](https://github.com/opensourceBIM/BIMserver/releases/download/1.3.2-FINAL-2014-08-21/bimserver-1.3.2-FINAL-2014-08-21.war) -O ROOT.war | Download the latest BIMserver (Make sure you replace this with the latest version!) |

After this command, Tomcat 7 should start unpacking the downloaded war file in a directory called ROOT. After a while you should be able to connect to the BIMserver with a browser on your http://[YOUR DOMAIN]:[CONFIGURED PORT]. The page you will see should be showing the version of BIMserver and the status (should be NOT_SETUP if this is your first install). Continue to [Setup][setup] for further configuration.

When things are not working, you can look in the Tomcat 7 log file: /opt/tomcat7/logs/catalina.out and the BIMserver log file: /var/bimserver/home/logs/bimserver.log.

# Installing an STMP server

You only have to do this if you do not already have an accessible SMTP server running in your network or with your ISP. Remember running your own SMTP server is a security/spam risk if you don't know how to properly install/maintain it.

> It really is way easier to use a third party for email, usually can use those for free, mailgun works very nice.

```
apt-get install postfix
```

Select "Internet server", use a real domain name that is pointing to your server's IP address.

## Alternative to running an SMTP server

Since BIMserver 1.3 there is an alternative for running your own mailserver. You can now choose a lot more options such as the protocol (SMTP/SMTPS), username, password and port. This allows for using a 3rd party e-mail service which is probably a good choice (and doesn't have to cost extra). Examples of these providers are [Mailgun](http://www.mailgun.com), [Amazon SES](http://aws.amazon.com/ses) and [Sendgrid](http://sendgrid.com).

# Running on ports below 1024

The tomcat7 user has no rights to bind to ports below 1024 (only root can), to make the server available on port 80 (the default HTTP port), you can use iptables (you might have to install the package "iptables"):

```
apt-get install iptables
iptables -t nat -A PREROUTING -i eth0 -p tcp --dport 80 -j REDIRECT --to-port 8080
```
to store these settings:
```
iptables-save
```

An alternative with IP addresses (useful when multiple IP adresses are being used on 1 server)
```
iptables -t nat -d [YOURIP] -A PREROUTING -i eth0 -p tcp --dport 80 -j DNAT --to-destination [YOURIP]:8080
```

To also forward the local ports do
```
iptables -t nat -I OUTPUT --src 0/0 --dst 127.0.0.1 -p tcp --dport 80 -j REDIRECT --to-ports 8080
iptables -t nat -I OUTPUT --src 0/0 --dst [PUBLIC IP] -p tcp --dport 80 -j REDIRECT --to-ports 8080
```

> Note: You might also have to install iptables-persistent


> If you are running on Amazon or another Cloud provider, make sure you enable port 80 (or whatever port you redirect) on their firewall as well.

# Multicast on loopback interface

On ubuntu you can apparently run into this error (thanks https://github.com/hlg):

When running BIMserver locally on Ubuntu without any internet connection using the JAR starter, then server startup fails with:

```
java.net.SocketException: No such device
  at java.net.PlainDatagramSocketImpl.join(Native Method)
  at java.net.AbstractPlainDatagramSocketImpl.join(AbstractPlainDatagramSocketImpl.java:178)
  at java.net.MulticastSocket.joinGroup(MulticastSocket.java:319)
  at org.apache.cxf.transport.udp.UDPDestination.activate(UDPDestination.java:168)
  ... 35 more
```
This is due to the loopback interface not having multicast traffic allowed by default. This can be enabled with:

```
sudo ifconfig lo multicast
sudo route add -net 224.0.0.0 netmask 240.0.0.0 dev lo
```

The solution was found at ubuntuforms.org. Please add this information to the BIMserver wiki.