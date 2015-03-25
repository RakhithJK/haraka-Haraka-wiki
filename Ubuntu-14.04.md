# Install [Ubuntu 14.04 LTS](http://www.ubuntu.com/download)
* download an [Ubuntu Server ISO](http://releases.ubuntu.com/14.04.2/ubuntu-14.04.2-server-amd64.iso)
* install as appropriate for your [virtual] machine.
    * (I accepted all the defaults, assuming the Ubuntu packagers know better than I do what reasonable defaults are.)
* Software selection:
    * OpenSSH server

# Customize $EDITOR
Get rid of that awful nano default editor:
```sh
sudo update-alternatives --config editor
```

# Install Unbound
Mail servers need a fast, reliable and DNS server. [Unbound][unbound-site] is all of that and more.
```sh
apt-get install -y unbound
```

# Install ClamAV
[ClamAV][clamav-site] is a virus scanner. Haraka will use it via the [clamd][clamd-plugin] plugin.
```sh
apt-get install -y clamd
```



[unbound-site]: https://unbound.net
[clamav-site]: http://www.clamav.net/
[clamd-plugin]: https://github.com/baudehlo/Haraka/blob/master/plugins/clamd.js