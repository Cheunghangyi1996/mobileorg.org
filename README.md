# mobileorg.org
org-mobile-directory
 C-c C-x RET p 
 SPC q r
org-mobile-pull
org-mobile-push
MobileOrg (1)
MobileOrg (1)
org-mobile-directory
 index.org
 index.org
 /Volumes/private
 $ ls /Volumes/private/org
index.org meetings.org reference.org
https://www.example.com/private/org/index.org
DAVLockDB /usr/local/apache/var/DAVLock
<Location /org>
    DAV On  
    AuthType Basic
    AuthName "My Org Files"
    AuthUserFile /path/to/htpasswd-file
    <Limit GET PUT POST DELETE PROPFIND PROPPATCH MKCOL COPY MOVE LOCK UNLOCK>
        Require valid-user
    </Limit>
</Location>
#!/bin/sh

# on ubuntu: need some utils & dev libs
sudo apt-get install apache2-utils openssl libssl-dev libpcre3-dev
 
# compile nginx
cd /tmp
curl http://nginx.org/download/nginx-0.7.64.tar.gz | tar xz
cd nginx*
./configure --with-http_ssl_module --with-http_dav_module \
  --prefix=$HOME/nginx
make && make install
 
# generate an htpasswd file
htpasswd -c ~/.htpasswd $(whoami)
  
# ssl
openssl genrsa 1024 > ~/nginx/conf/server.key
openssl req -new -x509 -nodes -sha1 -days 365 \
    -key ~/nginx/conf/server.key > ~/nginx/conf/server.crt
  
# configure
cat > ~/nginx/conf/nginx.conf <<EOF
events {
    worker_connections 1024;
}
http {
   include mime.types;
   default_type application/octet-stream;
   ssl_certificate server.crt;
   ssl_certificate_key server.key;
   auth_basic "Restricted";
   auth_basic_user_file $HOME/.htpasswd;
   dav_methods put delete mkcol copy move;
   dav_access user:rw;
   create_full_put_path on;
   server {
       listen 1080;
       listen 1443 ssl;
       location ~ ^/org(/.*)?$ {
           alias $HOME/org/mobile\$1;
       }
   }
}
EOF
 
# now you can start nginx
~/nginx/sbin/nginx
 
# and then sync w/ org-mobile-push/pull & mobileorg sync
# URL: https://<my-nginx-ip-addr>:1443/org/index.org
# and your username and password you used above for htpasswd
index.org
index.org
first.org
second.org
third.org
third.org
* [[file:first.org][An Org file I like]]
* [[file:second.org][Another Org file I like]]
  This is a [[file:third.org][link]] in the body text.
  * Some text
* [[file:third.org][Link to third.org]]
* second.org
* third.org
* checksums.dat
* index.org
* org-mobile-push
* org-mobile-push
* checksums.dat
* $ md5sum * >checksums.dat
$ cat checksums.dat
2b00042f7481c7b056c4b410d28f33cf  first.org
41930d894e1a4c2353b85d0b8d96f381  index.org
e5b12e4697d09fa9757d3dc6fcaa5c5b  second.org
05eaf1239d84508477cda9d0fa86b1a1  third.org
  find . -name "*.org" -type f -print | sed 's/^\.\///' | xargs md5sum >checksums.dat
  md5sum
  md5
  shasum
  sha1sum
  ;; Enable encryption
(setq org-mobile-use-encryption t)
;; Set a password
(setq org-mobile-encryption-password "mypassword")
org-mobile-push
mobileorg.org
