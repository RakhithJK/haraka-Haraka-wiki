# Install [Ubuntu 14.04 LTS][ubuntu-dl]
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
Mail servers need a fast, reliable and DNS server. [Unbound][unbound-site] is all of that and more.
```sh
apt-get install -y unbound
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
sed -i.bak -e 's/;nodes=cpus/nodes=cpus/' $HARAKA_CONF/smtp.ini
sed -i.bak -e 's/;daemonize=true/daemonize=true/' $HARAKA_CONF/smtp.ini
```

### Configure Haraka startup
```sh
export HARAKA_INSTALL=/usr/local/lib/node_modules/Haraka
cp $HARAKA_INSTALL/contrib/haraka.conf /etc/init/
sed -i.bak -e 's/\/var\/haraka\/fwdmx/\/etc\/haraka/' /etc/init/haraka.conf
initctl start haraka
```

### Log to syslog
```sh
sed -i.bak -e 's/# log.syslog/log.syslog/' $HARAKA_CONF/plugins
sed -i.bak -e 's/always_ok=false/always_ok=true/' $HARAKA_CONF/log.syslog.ini
```

### Enable TLS/SSL
```sh
openssl req -x509 -nodes -days 2190 -newkey rsa:2048 \
   -keyout $HARAKA_CONF/tls_key.pem -out $HARAKA_CONF/tls_cert.pem
sed -i.bak -e 's/# tls/tls/' $HARAKA_CONF/plugins
```

## Install ClamAV
[ClamAV][clamav-site] is a virus scanner. Haraka will use it via the [clamd][clamd-plugin] plugin.
```sh
apt-get install -y clamav-daemon
dpkg-reconfigure clamav-base
```
### clamav non-default settings
* Socket type: TCP
* System logger: Yes
```sh
service clamav-daemon start
perl -pi -e 's/^rcpt_to.in_host_list$/rcpt_to.in_host_list\n\clamd' $HARAKA_CONF/plugins
```

## Install SpamAssassin
[SpamAssassin][spamd-site] is a spam scanning engine. It's written in perl, needs lots of resources, but is still very helpful. It is called via the [spamassassin][spamd-plugin] plugin.
```sh
apt-get install -y spamassassin
sed -i.bak -e 's/ENABLED=0/ENABLED=1/' /etc/default/spamassassin
sed -i.bak -e 's/CRON=0/CRON=1/' /etc/default/spamassassin
update-rc.d spamassassin enable
service spamassassin start
perl -pi -e 's/^rcpt_to.in_host_list$/rcpt_to.in_host_list\n\nspamassassin' $HARAKA_CONF/plugins
```

### Enable connection info plugins
Enable FCrDNS, SPF, bounce, data.headers, URIBL, and karma plugins.
```sh
perl -pi -e 's/^access$/access\nconnect.geoip\nconnect.fcrdns/' $HARAKA_CONF/plugins
perl -pi -e 's/^mail_from.is_resolvable$/mail_from.is_resolvable\nspf/' $HARAKA_CONF/plugins
perl -pi -e 's/^rcpt_to.in_host_list$/rcpt_to.in_host_list\n\n# DATA\nbounce\ndata.headers\ndata.uribl\nclamd\nkarma/' $HARAKA_CONF/plugins
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
perl -pi -e 's/^access$/access\nconnect.p0f/' $HARAKA_CONF/plugins
```

### GeoIP
Enable GeoIP location lookups for mail connections.
```sh
npm install -g maxmind-geolite-mirror
mkdir -p /usr/local/share/GeoIP
/usr/local/bin/maxmind-geolite-mirror
ln -s /usr/local/bin/maxmind-geolite-mirror /etc/cron.weekly/
perl -pi -e 's/^access$/access\nconnect.geoip/' $HARAKA_CONF/plugins
```

### More
```sh
npm install -g ws express
```


[ubuntu-dl]: http://www.ubuntu.com/download
[ubuntu-iso]: http://releases.ubuntu.com/14.04.2/ubuntu-14.04.2-server-amd64.iso
[unbound-site]: https://unbound.net
[clamav-site]: http://www.clamav.net/
[clamd-plugin]: https://github.com/baudehlo/Haraka/blob/master/plugins/clamd.js
[spamd-site]: https://spamassassin.apache.org
[spamd-plugin]: https://github.com/baudehlo/Haraka/blob/master/plugins/spamassassin.js