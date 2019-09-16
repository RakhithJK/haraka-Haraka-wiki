# Future Plans / TODO
* [ ] [[Outbound Improvements]]
- [x] Support [RFC3464](http://tools.ietf.org/html/rfc3464) in bounce messages [#1189](https://github.com/haraka/Haraka/issues/1189)
- [ ] Decode Short URLs in data.uribl.js and test the destination URL instead
- [x] [DKIM verifier](https://github.com/haraka/Haraka/blob/master/plugins/dkim_verify.js)
- [ ] ditch vm.runInNewContext
- [ ] Migrate to Stream3 streams internally [#1323](https://github.com/haraka/Haraka/issues/1323)
- TLS
    - [X] Don't require TLS certificate pair when acting as an SMTP client. #1324
    - [ ] add TLS DANE support [#779](https://github.com/haraka/Haraka/issues/779)
    - [ ] support for MTA-STS (Strict Transport Security) [#2070](https://github.com/haraka/Haraka/issues/2070)
    - [ ] remove plugins/tls, consolidate to tls_socket [#2062](https://github.com/haraka/Haraka/issues/2062)
    - [x] permit TLS cert paths to be fully qualified [#2521](https://github.com/haraka/Haraka/issues/2521)  [#46](/haraka/haraka-config/issues/46)
- AUTH
    - [ ] Support AUTH SCRAM [#1214](/haraka/Haraka/issues/1214)
    - [ ] Support for multiple AUTH plugins [#1462](/haraka/Haraka/issues/1462)
    - [ ] auth-trap unrecognized failure [#2378](/haraka/Haraka/issues/2378)
- [ ] support for ARC [#2283](https://github.com/haraka/Haraka/issues/2283)
- [ ] native DMARC plugin
- [ ] Remove HTTP server to haraka-httpd [#2394](https://github.com/haraka/Haraka/issues/2394)
- [ ] Fuzz based testing [#2134](https://github.com/haraka/Haraka/issues/2134)
- [ ] Support proxy protocol v2 [#2601](https://github.com/haraka/Haraka/issues/2601)

# Ideas / Maybe Todo
- [ ] Use whois data to correlate ASNs to orgs [#1008](https://github.com/haraka/Haraka/issues/1008)
- [ ] Save the SMTP conversation [#945](https://github.com/haraka/Haraka/issues/945)
- [ ] Domain Reputation Database [#696](https://github.com/haraka/Haraka/issues/696)
- [ ] Outbound queue with rabbit [#1311](https://github.com/haraka/Haraka/issues/1311)
- [ ] CLI mode for plugins [#1220](https://github.com/haraka/Haraka/issues/1220)
- [ ] permit plugins to fork workers [#2099](https://github.com/haraka/Haraka/issues/2099)

# Feature Requests

- [x] custom HELO/EHLO messages [#1532](https://github.com/haraka/Haraka/issues/1532)
- [ ] cherry picked log levels [#1488](https://github.com/haraka/Haraka/issues/1488)
- [ ] reduced logging and more logging consistency [#2434](/haraka/Haraka/issues/2434)
- [ ] DKIM: sign any message with a "default" key [#1548](https://github.com/haraka/Haraka/issues/1548)
- [ ] DNSBL: add configurable timeouts [#2436](https://github.com/haraka/Haraka/issues/2436)