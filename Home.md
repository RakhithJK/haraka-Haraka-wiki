Haraka is an SMTP server built with [node.js](http://nodejs.org/).

Haraka provides extensibility via a plugin architecture with built-in support for plugins that call SpamAssassin, DNS Blocklists, provide authentication, and many more.

# Install Guides

* [[Ubuntu 14.04]]
* [FreeBSD 10](http://www.tnpi.net/wiki/MT6_SMTP)

# How To
* [[Google Safe Browsing]]
* [Require TLS](Setting up TLS with CA certificates)
* [Configure my Editor](Editor Settings)
* [Contribute](Contributing)
* [Roll a Release](Release Process)

# Future Plans / TODO
* [ ] [[Outbound Improvements]]
- [x] Support [RFC3464](http://tools.ietf.org/html/rfc3464) in bounce messages (#1189)
- [ ] Decode Short URLs in data.uribl.js and test the destination URL instead
- [x] [DKIM verifier](https://github.com/haraka/Haraka/blob/master/plugins/dkim_verify.js)
- [ ] ditch vm.runInNewContext
- [ ] Migrate to Stream3 streams internally #1323
- [X] Don't require TLS certificate pair when acting as an SMTP client. #1324
- [ ] Support AUTH SCRAM [#1214](https://github.com/haraka/Haraka/issues/1214)

# Ideas / Maybe Todo
- [ ] Use whois data to correlate ASNs to orgs [#1008](https://github.com/haraka/Haraka/issues/1008)
- [ ] Save the SMTP conversation [#945](https://github.com/haraka/Haraka/issues/945)

# Feature Requests

- [ ] cherry picked log levels [#1488](https://github.com/haraka/Haraka/issues/1488)

# Additional Resources
* [[FAQ / Help / Troubleshooting]]
* [[Parsing Haraka Logs]]
* [[Other Plugins]]