# Nginx

## Installation
```bash
# apt-get install -y nginx
```

## Usage
**Set your /etc/nginx/nginx.conf**
```conf
user www-data www-data;
worker_processes 1;
pid /run/nginx.pid;

events {
	worker_connections 1024;
}

http {
	##
	# Mime Types Settings
	#

	include      /etc/nginx/mime.types;
	default_type application/octet-stream;

	##
	# Log Settings
	##

	log_format main '$remote_addr - $remote_user [$time_local] '
			'"$request" $status  $body_bytes_sent "$http_referer" '
			'"$http_user_agent" "$http_x_forwarded_for"';

	##
	# Basic Settings
	##

	sendfile on;
	tcp_nopush on;
	tcp_nodelay on;
	keepalive_requests 10;
	keepalive_timeout 10;

	server_tokens off;
	client_header_timeout 10;
	client_body_timeout 10;
	ignore_invalid_headers on;
	send_timeout 10;
	server_name_in_redirect off;

	##
	# SSL Settings
	##

	ssl_protocols TLSv1 TLSv1.1 TLSv1.2; # Dropping SSLv3, ref: POODLE
	ssl_prefer_server_ciphers on;

	##
	# Logging Settings
	##

	access_log /var/log/nginx/access.log;
	error_log /var/log/nginx/error.log;

	##
	# Gzip Settings
	##

	gzip on;
	gzip_disable "msie6";
	gzip_proxied any;
	gzip_comp_level 6;
	gzip_http_version 1.0;
	gzip_types text/plain text/css application/json
		   application/javascript text/xml application/xml
		   application/xml+rss text/javascript
		   image/svg+xml;

	##
	# Virtual Host Configs
	##

	include /etc/nginx/conf.d/*.conf;
	include /etc/nginx/sites-enabled/*;
}
```

**Create your virtual host (like this for example)**
```conf
upstream app {
        server localhost:3000;
}

server {
        listen               80;
        server_name          your-external-ip escritoriovirtual.lk8.com.br;
        root                 /home/escritoriovirtual/lk8-bonus/current/public;
        index                index.html index.html;
        client_max_body_size 10M;
        error_page           500 502 503 504 /500.html;

        location ~ (\.(php|aspx|asp)|myadmin) {
                return 444;
        }

        location ~ ^/assets/ {
                gzip_static on;
                expires     max;
                add_header  Cache-Control public;
                access_log  off;
        }

        location / {
                try_files $uri @app;
        }

        location @app {
                proxy_set_header X-Real-IP         $remote_addr;
                proxy_set_header X-Forwarded-For   $proxy_add_x_forwarded_for;
                proxy_set_header Host              $http_host;
                proxy_set_header X-FORWARDED_PROTO $scheme;
                proxy_redirect   off;
                proxy_pass       http://app;
                break;
        }
}
```
