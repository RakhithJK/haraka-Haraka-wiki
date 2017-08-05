
To install a signed (and possibly chained) SSL certificate, the contents of **config/tls_cert.pem** need to be:

1. The CA signed SSL cert
2. Any intermediate certificates
3. The CA root certificate

If they are placed in the file in that order, it will work fine.

See also [Issue #365](https://github.com/haraka/Haraka/issues/365)

### example

`cat mail.example.com.crt intermediary_cert.crt ca-cert.crt > config/tls_cert.pem`