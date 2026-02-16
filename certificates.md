1. **End of a certificate**
   
```bash
@/etc/ssl/etcd/ssl # cat ca.pem | openssl x509 -enddate -noout
notAfter=Jun 16 17:11:53 2125 GMT
```

2. **Get the certificate from an url**
   
```bash
openssl s_client -showcerts -servername valid-isrgrootx2.letsencrypt.org -connect valid-isrgrootx2.letsencrypt.org:443 2>/dev/null </dev/null |  sed -ne '/-BEGIN CERTIFICATE-/,/-END CERTIFICATE-/p' > root-ca
```
