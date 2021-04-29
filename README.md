# nginx_sm

## an enhanced version with TLCP/GMTLS protocol support

## Compile Guide

1. Install GmSSL

<pre>
wget https://github.com/guanzhi/GmSSL/archive/master.zip
unzip master.zip
cd GmSSL-master
./config --prefix=/usr/local/gmssl --openssldir=/usr/local/gmssl no-shared
make
make install
</pre>

NOTICE: if glob error is encounted, edit file "Configure & test/build.info", change 'File::Glob' => qw/:glob/;

2. Install Nginx_sm

<pre>
apt-get install libpcre3 libpcre3-dev zlib1g zlib1g-dev

./auto/configure  \
    --prefix=/usr/local/nginxsm \
    --with-http_ssl_module \
    --with-http_realip_module \
    --with-http_addition_module \
    --with-http_sub_module \
    --with-http_dav_module \
    --with-http_flv_module \
    --with-http_mp4_module \
    --with-http_gunzip_module \
    --with-http_gzip_static_module \
    --with-http_random_index_module \
    --with-http_secure_link_module \
    --with-http_stub_status_module \
    --with-http_auth_request_module \
    --with-threads \
    --with-stream \
    --with-stream_ssl_module \
    --with-http_slice_module \
    --with-mail \
    --with-mail_ssl_module \
    --with-file-aio \
    --with-http_v2_module \
    --with-openssl=/usr/local/gmssl

make 
make install
</pre>

3. Config Nginx_sm

# HTTPS server
server {
    listen       1443 ssl;
    server_name  localhost;

    # ssl_protocols GMTLS;
    ssl_certificate      /root/sig.pem;
    ssl_certificate_key  /root/sig.key;
    ssl_certificate      /root/enc.pem;
    ssl_certificate_key  /root/enc.key;
    ssl_protocols TLSv1.2 TLSv1.3 GMTLS;
    ssl_certificate      /root/root.pem;

    ssl_session_cache    shared:SSL:1m;
    ssl_session_timeout  5m;

    ssl_ciphers "ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:DHE-RSA-CHACHA20-POLY1305:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES128-GCM-SHA256:ECDHE-SM2-WITH-SMS4-GCM-SM3:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA384:DHE-RSA-AES256-SHA256:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA256:DHE-RSA-AES128-SHA256:ECDHE-SM2-WITH-SMS4-SHA256:ECDHE-SM2-WITH-SMS4-SM3:ECDHE-ECDSA-AES256-SHA:ECDHE-RSA-AES256-SHA:DHE-RSA-AES256-SHA:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES128-SHA:DHE-RSA-AES128-SHA:RSA-PSK-AES256-GCM-SHA384:DHE-PSK-AES256-GCM-SHA384:RSA-PSK-CHACHA20-POLY1305:DHE-PSK-CHACHA20-POLY1305:ECDHE-PSK-CHACHA20-POLY1305:AES256-GCM-SHA384:PSK-AES256-GCM-SHA384:PSK-CHACHA20-POLY1305:RSA-PSK-AES128-GCM-SHA256:DHE-PSK-AES128-GCM-SHA256:AES128-GCM-SHA256:PSK-AES128-GCM-SHA256:AES256-SHA256:AES128-SHA256:ECDHE-PSK-AES256-CBC-SHA384:ECDHE-PSK-AES256-CBC-SHA:SRP-RSA-AES-256-CBC-SHA:SRP-AES-256-CBC-SHA:RSA-PSK-AES256-CBC-SHA384:DHE-PSK-AES256-CBC-SHA384:RSA-PSK-AES256-CBC-SHA:DHE-PSK-AES256-CBC-SHA:AES256-SHA:PSK-AES256-CBC-SHA384:PSK-AES256-CBC-SHA:ECDHE-PSK-AES128-CBC-SHA256:ECDHE-PSK-AES128-CBC-SHA:SRP-RSA-AES-128-CBC-SHA:SRP-AES-128-CBC-SHA:RSA-PSK-AES128-CBC-SHA256:DHE-PSK-AES128-CBC-SHA256:RSA-PSK-AES128-CBC-SHA:DHE-PSK-AES128-CBC-SHA:SM9-WITH-SMS4-SM3:SM9DHE-WITH-SMS4-SM3:SM2-WITH-SMS4-SM3:SM2DHE-WITH-SMS4-SM3:AES128-SHA:RSA-WITH-SMS4-SHA1:RSA-WITH-SMS4-SM3:PSK-AES128-CBC-SHA256:PSK-AES128-CBC-SHA";
    # ssl_ciphers  HIGH:!aNULL:!MD5;
    ssl_prefer_server_ciphers  on;

    location / {
        root   html;
        index  index.html index.htm;
    }
}
