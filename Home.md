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
* [[Outbound Improvements]]
- Support [RFC3464](http://tools.ietf.org/html/rfc3464) in bounce messages
- Decode Short URLs in data.uribl.js and test the destination URL instead
- ~~[DKIM verifier](https://github.com/baudehlo/Haraka/blob/master/plugins/dkim_verify.js)~~


# Additional Resources
* [[Parsing Haraka Logs]]
* [[Other Plugins]]