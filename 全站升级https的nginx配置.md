##nginx升级到全站https

<pre>
 if ($server_port = 80) {

        return 301 https://$server_name$request_uri;

        }
        if ($scheme = http) {

        return 301 https://$server_name$request_uri;

        }
        error_page 497 https://$server_name$request_uri;

         ssl on;
        ssl_certificate      keys/m-traininguat.akzonobel.com.crt;
        ssl_certificate_key   keys/m-traininguat.akzonobel.com.key;

        ssl_session_timeout  5m;
        ssl_prefer_server_ciphers on;
        ssl_session_cache shared:SSL:10m;
        ssl_dhparam keys/dhparam.pem;
        ssl_protocols   TLSv1 TLSv1.1 TLSv1.2;

        ssl_ciphers  "EECDH+AESGCM:EDH+AESGCM:AES256+EECDH:AES256+EDH";
        ssl_ecdh_curve secp384r1;
        add_header Strict-Transport-Security "max-age=63072000; includeSubdomains; preload";
        add_header X-Frame-Options DENY;
        add_header X-Content-Type-Options nosniff;

</pre>