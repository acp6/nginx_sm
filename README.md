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
