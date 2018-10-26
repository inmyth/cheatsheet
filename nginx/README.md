## SSL Installation

References
- https://www.digitalocean.com/community/tutorials/how-to-create-a-self-signed-ssl-certificate-for-nginx-in-ubuntu-16-04
- https://qiita.com/kojirof/items/66d956609a45b04169f3

### CSR Generation

Use OpenSSL to generate private key and csr file
```
openssl req -new -newkey rsa:2048 -nodes -keyout www.martiply.com.key -out www.martiply.com.csr
```
The content of csr is needed to create SSL certificates.

Put `.key` in `/etc/ssl/private/`

Put `.csr` in `/etc/ssl/certs/`

### crt Bundle

Cat the certificates from COMODO, in order :
```
cat www_martiply_com.crt COMODORSADomainValidationSecureServerCA.crt COMODORSAAddTrustCA.crt >> www.martiply.com.ca-bundle.crt
```
Put this file in `/etc/ssl/certs/`

### DhParam

Generate with OpenSSL
```
openssl dhparam -out /etc/ssl/certs/dhparam.pem 2048
```


## Nginx Config

### .conf directories

All `.conf` files except `domain.conf` go to `/etc/nginx/`

`domain.conf` goes to `/etc/nginx/vhost.d` (create this dir first)

### SSL config

`ssl.conf` contains
- path to bundled crt file
- path to private key `.key` file
- path to dhparam

### Domain config
- Refer to the martiply.conf which already provides
    - redirect from bare IP
    - upstream server where the real web server is running
    - redirect from bare domain to www
    - redirect to https

### http config
- comment out this line `include /etc/nginx/sites-enabled/*;` by adding `#`
    - https://stackoverflow.com/questions/15335002/nginx-simple-proxy-pass-to-localhost-not-working    

### Proxy pass and path

When passing request to another server, pay attention to the path
```
location /abc {
  proxy_pass http://upstream
}

```
Server at upstream will receive the request with its original path "/abc..."
To remove this path, use /
```
location /abc/ {
  proxy_pass http://upstream/
}
```
https://stackoverflow.com/questions/15353935/correct-proxy-path-in-nginx-conf (comment of the first answer)

### Allow / deny access
- create folder `/etc/nginx/includes`
- create any text file `whitelist` and put
```
allow 1.2.3.4; # Allow a single remote host
deny all; # Deny everyone else
```
- in `xxx.cfg`
```
location / {
  include includes/whitelist;
  proxy_pass ...
}
```
- restart

https://support.hypernode.com/knowledgebase/blocking-allowing-ip-addresses-in-nginx/
https://www.linkedin.com/pulse/tutorial-whitelisting-ip-addresses-nginx-easy-way-david-gracie/
