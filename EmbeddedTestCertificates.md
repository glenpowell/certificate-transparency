# Introduction #

This page describes how to generate a Certificate with an embedded SCT utilizing the code available in a repository.

# Requirements #
  * OpenSSL installed
  * Built CT binaries.

# Steps #
## Generating a new CA cert ##
```
cd src/test
source generate_certs.sh
set +e # So that bash would not exit on errors
mkdir -p /tmp/certs
make_ca_certs /tmp/certs /tmp ca
ca_setup /tmp/certs ca
```
## Adding CA cert to your user's trusted roots ##
On Linux, the following should work:
```
certutil -d sql:$HOME/.pki/nssdb -A -t "C,," -n ct_test -i ca-cert.pem
```
On OS X, start `Keychain Access`, and import ca-cert.pem via "File -> Import Items...".
## Starting a log locally ##
For simplicity, use the log's existing test keys under `src/test/testdata`:
```
src/test$ ./run_log_server.sh /tmp/logstore <path to ca-cert.pem>
```
## Generating a certificate ##
The crux of the matter - creating a certificate with a specified commonName and an embedded SCT:
```
cd src/test
source generate_certs.sh
set +e # So that bash would not exit on errors
make_embedded_cert /tmp/ca <name for output certificate file> ca <lop ip:port> -http_log false false testdata/ct-server-key-public.pem <desired commonName>
```
## Starting Apache with generated certificate ##
Assuming the certificate was named `myname`, the following should work:
```
cd src/test
mkdir -p apache_tmp
cp /tmp/myname-cert.pem /tmp/myname-key.pem apache_tmp
cp httpd-devel.conf httpd-common.conf apache_tmp
ln -s /usr/lib/apache2/modules/mod_ssl.so apache_tmp
```
Edit `httpd-common.conf` to have the following line:
```
LoadModule ssl_module mod_ssl.so
```
And `httpd-devel.conf` to point to your certificates, exact hostname and port. An example config would look like this:
```
Include httpd-common.conf
Listen 8123
ServerName myserver.someplace.com

<VirtualHost *:8123>
ServerName https://myserver.someplace.com:8123
DocumentRoot contentroot
SSLCertificateFile myname-cert.pem
SSLCertificateKeyFile myname-key.pem
SSLEngine on
</VirtualHost>
```

Finally, start apache:
```
apachectl -d $PWD/apache_tmp -f httpd-devel.conf -k start
```

## Verifying the generated certificate ##
Start by preparing the ca-cert to be read by the CT openssl client:
```
openssl x509 -in ca-cert.pem -hash -noout
```

Then take the result and link:
```
ln -s ca-cert.pem <result>.0
```

Finally, run the ct client
```
src$ ./client/ct connect -alsologtostderr -ssl_server <http server IP> --ssl_server_port <http server port> -ct_server_public_key test/testdata/ct-server-key-public.pem -ssl_client_trusted_cert_dir <path to ca-cert and link>
```