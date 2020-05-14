# Install [Ubuntu 14.04 LTS][ubuntu-dl]
Written and tested against Haraka 2.6.0.

* download an [Ubuntu Server ISO][ubuntu-iso]
* install as appropriate for your [virtual] machine.
    * (I accepted all the defaults, assuming the Ubuntu packagers know better than I do what reasonable defaults are.)
* Software selection:
    * OpenSSH server

## Apply updates
```sh
apt-get update
```

## Customize $EDITOR
Get rid of that awful nano default editor:
```sh
sudo update-alternatives --config editor
```

## Install Unbound
Mail servers need a fast and reliable DNS server. [Unbound][unbound-site] is all that and more.
```sh
apt-get install -y unbound
```

## Install Node v6

```
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
sudo apt-get install -y nodejs
```

# Install Haraka

## Haraka prereqs

```sh
apt-get install -y nodejs npm redis-server
ln -s /usr/bin/nodejs /usr/bin/node
```

## Haraka
```sh
npm install -g Haraka
haraka -i /etc/haraka
export HARAKA_CONF=/etc/haraka/config
<<<<<<< HEAD
```

### Enable cluster mode
```sh
sed -i.bak -e 's/;nodes=cpus/nodes=cpus/' $HARAKA_CONF/smtp.ini
```

### Log to syslog
This is not required, but if you don't switch to syslog, make sure to set up proper log rotation for haraka.log, **before** it consumes all your disk.
```sh
sed -i.bak -e 's/# log.syslog/log.syslog/' $HARAKA_CONF/plugins
sed -i.bak -e 's/always_ok=false/always_ok=true/' $HARAKA_CONF/log.syslog.ini
```

### Configure Haraka startup
The upstart config expects Haraka to have the daemonize plugin enabled. If you choose not to enable daemonize, then be sure to edit haraka.conf and comment out the `export fork` line.
```sh
sed -i.bak -e 's/;daemonize=true/daemonize=true/' $HARAKA_CONF/smtp.ini
export HARAKA_INSTALL=/usr/local/lib/node_modules/Haraka
cp $HARAKA_INSTALL/contrib/ubuntu-upstart/haraka.conf /etc/init/
initctl start haraka
```

### Enable TLS/SSL
If you have a $igned TLS certificate, install it here instead. See also [this note on CA certificates](https://github.com/haraka/Haraka/wiki/Setting%20up%20TLS%20with%20CA%20certificates).
=======

### Enable cluster mode
```sh
sed -i.bak -e 's/;nodes=cpus/nodes=cpus/' $HARAKA_CONF/smtp.ini
```

### Log to syslog
This is not required, but if you don't switch to syslog, make sure to set up proper log rotation for haraka.log, **before** it consumes all your disk.
```sh
sed -i.bak -e 's/# log.syslog/log.syslog/' $HARAKA_CONF/plugins
sed -i.bak -e 's/always_ok=false/always_ok=true/' $HARAKA_CONF/log.syslog.ini
```

### Configure Haraka startup
The upstart config expects Haraka to have the daemonize plugin enabled. If you choose not to enable daemonize, then be sure to edit haraka.conf and comment out the `export fork` line.
```sh
sed -i.bak -e 's/;daemonize=true/daemonize=true/' $HARAKA_CONF/smtp.ini
export HARAKA_INSTALL=/usr/local/lib/node_modules/Haraka
cp $HARAKA_INSTALL/contrib/haraka.conf /etc/init/
sed -i.bak -e 's/\/var\/haraka\/fwdmx/\/etc\/haraka/' /etc/init/haraka.conf
initctl start haraka
```

### Enable TLS/SSL
If you have a $igned TLS certificate, install it here instead. See also [this note on CA certificates](https://github.com/haraka/Haraka/wiki/Setting%20up%20TLS%20with%20CA%20certificates).
>>>>>>> add smtp_forward and queue setup
```sh
openssl req -x509 -nodes -days 2190 -newkey rsa:2048 \
   -keyout $HARAKA_CONF/tls_key.pem -out $HARAKA_CONF/tls_cert.pem
sed -i.bak -e 's/# tls/tls/' $HARAKA_CONF/plugins
```

### Install ClamAV
[ClamAV][clamav-site] is a virus scanner. Haraka will use it via the [clamd][clamd-plugin] plugin.
```sh
apt-get install -y clamav-daemon
dpkg-reconfigure clamav-base
```
#### clamav non-default settings
* Socket type: TCP
* System logger: Yes
```sh
service clamav-daemon start
sed -i.bak -e 's/#clamd/clamd/' $HARAKA_CONF/plugins
```

### Install SpamAssassin
[SpamAssassin][spamd-site] is a spam scanning engine. It's written in perl, needs lots of resources, but is still very helpful. It is called via the [spamassassin][spamd-plugin] plugin.
```sh
apt-get install -y spamassassin
sed -i.bak -e 's/ENABLED=0/ENABLED=1/' /etc/default/spamassassin
sed -i.bak -e 's/CRON=0/CRON=1/' /etc/default/spamassassin
update-rc.d spamassassin enable
service spamassassin start
sed -i.bak -e 's/#spamassassin/spamassassin/' $HARAKA_CONF/plugins
```

### Enable connection info plugins
Enable FCrDNS, SPF, bounce, URIBL, and karma plugins.
```sh
sed -i.bak -e 's/# connect.fcrdns/connect.fcrdns/' $HARAKA_CONF/plugins
sed -i.bak -e 's/#spf/spf/' $HARAKA_CONF/plugins
sed -i.bak -e 's/#bounce/bounce/' $HARAKA_CONF/plugins
sed -i.bak -e 's/#karma/karma/' $HARAKA_CONF/plugins
sed -i.bak -e 's/#data.uribl/data.uribl/' $HARAKA_CONF/plugins
```

### p0f
Ubuntu installs p0f 2 and the Haraka plugin only supports version 3. You'll have to manually install p0f v3 to use it.

```sh
apt-get install -y p0f libpcap-dev
mkdir ~/p0f && cd ~/p0f
curl -O http://lcamtuf.coredump.cx/p0f3/releases/p0f-3.08b.tgz
tar -xzf p0f-3.08b.tgz 
cd p0f-3.08b/
./build.sh
cp p0f /usr/sbin/p0f 
cp p0f.fp /etc/p0f/p0f.fp
cp $HARAKA_INSTALL/contrib/ubuntu-upstart/p0f.conf /etc/init/
initctl start p0f
sed -i.bak -e 's/# connect.p0f/connect.p0f/' $HARAKA_CONF/plugins
```

### GeoIP
Enable GeoIP location lookups for mail connections.
```sh
npm install -g maxmind-geolite-mirror
mkdir -p /usr/local/share/GeoIP
/usr/local/bin/maxmind-geolite-mirror
ln -s /usr/local/bin/maxmind-geolite-mirror /etc/cron.weekly/
sed -i.bak -e 's/# connect.geoip/connect.geoip/' $HARAKA_CONF/plugins
```

### http-server
```sh
apt-get install -y git
npm install -g ws express bower grunt-cli
mkdir -p /etc/haraka/http/html
cd /etc/haraka/http
export GITHUB_HTTP="https://raw.githubusercontent.com/msimerson/Haraka/http-server"
curl -o html/index.html $GITHUB_HTTP/http/html/index.html
curl -O $GITHUB_HTTP/http/package.json
curl -O $GITHUB_HTTP/http/bower.json
curl -O $GITHUB_HTTP/http/Gruntfile.js
npm install
grunt
curl -o /usr/local/lib/node_modules/Haraka/server.js $GITHUB_HTTP/server.js
curl -o $HARAKA_CONF/http.ini https://raw.githubusercontent.com/msimerson/Haraka/http-server/config/http.ini
sed -i.bak -e 's/; listen=/listen=/' $HARAKA_CONF/http.ini
mv html ../
```

### watch
Watch depends on the http-server being installed.
```sh
TODO
```


# Configure RCPT
Haraka needs to know who is accepts mail for. There are a variety of plugins (flat file, qmail-deliverabled, LDAP) that check recipient users and/or hosts against data sources to determine if Haraka should accept mail for them. The default plugin is [rcpt_to.in_host_list][plugin-host-list-doc] and matches on domain names.
```sh
echo 'my-domain-name.tld' > $HARAKA_CONF/host_list
```
Now Haraka will accept messages for anyone@my-domain-name.tld.

## Set up queue
Haraka does not include a mail store, which means you need to configure one. The default queue plugin is smtp_forward, which forwards the messages on to a mail store.
```sh
$EDITOR $HARAKA_CONF/smtp_forward.ini
```

# Test

From another host, send test messages:

```sh
swaks -server my.haraka.host -to valid@my-domain-name.tld -from my@valid.email
```


### watch
This plugin depends on having the http-server installed
```sh
...TODO...
```


# Configure RCPT
Haraka needs to know who is accepts mail for. There are a variety of plugins (flat file, qmail-deliverabled, LDAP) that check recipient users and/or hosts against data sources to determine if Haraka should accept mail for them. The default plugin is [rcpt_to.in_host_list][plugin-host-list-doc] and matches on domain names.
```sh
echo 'my-domain-name.tld' > $HARAKA_CONF/host_list
```
Now Haraka will accept messages for anyone@my-domain-name.tld.

## Set up queue
Haraka does not include a mail store, which means you need to configure one. The default queue plugin is smtp_forward, which forwards the messages on to a mail store.
```sh
$EDITOR $HARAKA_CONF/smtp_forward.ini
```

# Test

From another host, send test messages:

```sh
swaks -server my.haraka.host -to valid@my-domain-name.tld -from my@valid.email
```



[ubuntu-dl]: http://www.ubuntu.com/download
[ubuntu-iso]: http://releases.ubuntu.com/14.04.2/ubuntu-14.04.2-server-amd64.iso
[unbound-site]: https://unbound.net
[clamav-site]: http://www.clamav.net/
[clamd-plugin]: https://github.com/haraka/Haraka/blob/master/plugins/clamd.js
[spamd-site]: https://spamassassin.apache.org
[spamd-plugin]: https://github.com/haraka/Haraka/blob/master/plugins/spamassassin.js
[plugin-host-list-doc]: https://github.com/haraka/Haraka/blob/master/docs/plugins/rcpt_to.in_host_list.md
