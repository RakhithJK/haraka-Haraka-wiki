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
Note that any of these services can be installed in other [virtual] machines and accessed over the network. Assembling in such a fashion makes it very easy to scale your system by applying the necessary resources exactly where they're needed.

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
```

## Optional, for "special features"
```sh
npm install -g ws express
```


[unbound-site]: https://unbound.net
[clamav-site]: http://www.clamav.net/
[clamd-plugin]: https://github.com/baudehlo/Haraka/blob/master/plugins/clamd.js
[spamd-site]: https://spamassassin.apache.org
[spamd-plugin]: https://github.com/baudehlo/Haraka/blob/master/plugins/spamassassin.js