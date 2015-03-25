# Install [Ubuntu 14.04 LTS](http://www.ubuntu.com/download)
* download an [Ubuntu Server ISO](http://releases.ubuntu.com/14.04.2/ubuntu-14.04.2-server-amd64.iso)
* install as appropriate for your [virtual] machine.
    * (I accepted all the defaults, assuming the Ubuntu packagers know better than I do what reasonable defaults are.)
* Software selection:
    * OpenSSH server

## Customize $EDITOR
Get rid of that awful nano default editor:
```sh
sudo update-alternatives --config editor
```

# Prereq services
Note that any of these [optional] services can be installed and run in other [virtual] machines and accessed over the network. For large installs, where clustering is likely, isolating these services makes it easier to scale the system by applying additional resources exactly where they're needed.

## Install Unbound
Mail servers need a fast, reliable and DNS server. [Unbound][unbound-site] is all of that and more.
```sh
apt-get install -y unbound
```

## Install ClamAV
[ClamAV][clamav-site] is a virus scanner. Haraka will use it via the [clamd][clamd-plugin] plugin.
```sh
apt-get install -y clamav-daemon
```

## Install SpamAssassin
[SpamAssassin][spamd-site] is a spam scanning engine. It's written in perl, needs lots of resources, but is still very helpful. It is called via the [spamassassin][spamd-plugin] plugin.
```sh
apt-get install -y spamassassin
sed -i.bak -e 's/ENABLED=0/ENABLED=1/' /etc/default/spamassassin
sed -i.bak -e 's/CRON=0/CRON=1/' /etc/default/spamassassin
update-rc.d spamassassin enable
service spamassassin start
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
```

### Configure Haraka startup
```sh
export HARAKA_INSTALL=/usr/local/lib/node_modules/Haraka
sed -i.bak -e 's/\/var\/haraka\/fwdmx/\/etc\/haraka/' $HARAKA_INSTALL/contrib/haraka.conf
cp $HARAKA_INSTALL/contrib/haraka.conf /etc/init/
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

### Enable connection info plugins
Enable p0f, GeoIP, FCrDNS plugins.
```sh
perl -pi -e 's/^access$/access\nconnect.p0f\nconnect.geoip\nconnect.fcrdns/' $HARAKA_CONF/plugins
perl -pi -e 's/^mail_from.is_resolvable$/mail_from.is_resolvable\nspf/' $HARAKA_CONF/plugins
perl -pi -e 's/^rcpt_to.in_host_list$/rcpt_to.in_host_list\n\n# DATA\nbounce\ndata.headers\ndata.uribl\nclamd\nspamassassin\nkarma/' $HARAKA_CONF/plugins
```


## Other goodies, for extra special features
```sh
apt-get install -y p0f
/usr/sbin/p0f &
echo '@reboot /usr/sbin/p0f &' >> /etc/crontab

npm install -g ws express
```

### GeoIP
```sh
npm install -g maxmind-geolite-mirror
mkdir -p /usr/local/share/GeoIP
/usr/local/bin/maxmind-geolite-mirror
ln -s /usr/local/bin/maxmind-geolite-mirror /etc/cron.weekly/
```

[unbound-site]: https://unbound.net
[clamav-site]: http://www.clamav.net/
[clamd-plugin]: https://github.com/baudehlo/Haraka/blob/master/plugins/clamd.js
[spamd-site]: https://spamassassin.apache.org
[spamd-plugin]: https://github.com/baudehlo/Haraka/blob/master/plugins/spamassassin.js