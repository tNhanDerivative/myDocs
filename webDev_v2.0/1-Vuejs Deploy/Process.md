
# Previewing Locally
The dist directory is meant to be served by an HTTP server (unless you've configured publicPath to be a relative value), so it will not work if you open dist/index.html directly over file:// protocol. The easiest way to preview your production build locally is using a Node.js static file server, for example serve:

`npm install -g serve`
-s flag means serve it in Single-Page Application mode
which deals with the routing problem below
`serve -s dist`


# Nginx



```bash
server {
    server_name vuestore.nhan-thenoobmaster.com;

    charset utf-8;
    root    /var/www/html/vuestore/dist;
    index   index.html;

    location / {
        root /var/www/html/vuestore/dist;
        try_files $uri /index.html;
    }
    error_log  /var/log/nginx/vue-app-error.log;
    access_log /var/log/nginx/vue-app-access.log;

    listen 443 ssl; # managed by Certbot
    ssl_certificate /etc/letsencrypt/live/vuestore.nhan-thenoobmaster.com/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/vuestore.nhan-thenoobmaster.com/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot

}

```



