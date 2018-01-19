Haraka is an SMTP server built with [node.js](http://nodejs.org/).

Haraka provides extensibility via a plugin architecture with built-in support for plugins that call SpamAssassin, DNS Blocklists, provide authentication, and many more.

# Install Guides

* [[Ubuntu 14.04]]
* [FreeBSD 10](http://www.tnpi.net/wiki/MT6_SMTP)

# How To
* [[Google Safe Browsing]]
* [Require TLS](Setting-up-TLS-with-CA-certificates)
* [Configure my Editor](https://github.com/haraka/Haraka/wiki/Editor-Settings)
* [Contribute](Contributing)
* [Roll a Release](Release-Process)
* [Upgrade](Upgrade)
* [Write a Plugin](Write-a-Plugin)

# Future Plans / TODO
* [ ] [[Outbound Improvements]]
- [x] Support [RFC3464](http://tools.ietf.org/html/rfc3464) in bounce messages (#1189)
- [ ] Decode Short URLs in data.uribl.js and test the destination URL instead
- [x] [DKIM verifier](https://github.com/haraka/Haraka/blob/master/plugins/dkim_verify.js)
- [ ] ditch vm.runInNewContext
- [ ] Migrate to Stream3 streams internally #1323
- [X] Don't require TLS certificate pair when acting as an SMTP client. #1324
- [ ] Support AUTH SCRAM [#1214](https://github.com/haraka/Haraka/issues/1214)
- [ ] support for MTA-STS (Strict Transport Security) [#2070](https://github.com/haraka/Haraka/issues/2070)
- [ ] support for ARC [#2283](https://github.com/haraka/Haraka/issues/2283)

# Ideas / Maybe Todo
- [ ] Use whois data to correlate ASNs to orgs [#1008](https://github.com/haraka/Haraka/issues/1008)
- [ ] Save the SMTP conversation [#945](https://github.com/haraka/Haraka/issues/945)
- [ ] Domain Reputation Database [#696](https://github.com/haraka/Haraka/issues/696)
- [ ] Outbound queue with rabbit [#1311](https://github.com/haraka/Haraka/issues/1311)

# Feature Requests

- [ ] custom HELO/EHLO messages [#1532](https://github.com/haraka/Haraka/issues/1532)
- [ ] cherry picked log levels [#1488](https://github.com/haraka/Haraka/issues/1488)

# Additional Resources
* [[FAQ / Help / Troubleshooting]]
* [[Performance Tuning]]
* [[Parsing Haraka Logs]]
* [[Other Plugins]]

# Tutorials/Blog Posts
* [Creating Your Own E-Mail service with Haraka, PostgreSQL and AWS S3](http://thihara.github.io/Creating-E-Mail-Service-with-Haraka/)