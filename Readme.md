sudo apt update
sudo apt install nginx
<!-- vagrant@hellocloudnew:~$ cd /var/www/html/
vagrant@hellocloudnew:/var/www/html$ ls -la
total 12
drwxr-xr-x 2 root root 4096 Oct 12 06:28 .
drwxr-xr-x 3 root root 4096 Oct 12 06:28 ..
-rw-r--r-- 1 root root  612 Oct 12 06:28 index.nginx-debian.html
vagrant@hellocloudnew:/var/www/html$ cat index.nginx-debian.html 
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
vagrant@hellocloudnew:/var/www/html$  -->



<!-- vagrant@hellocloudnew:/var/www/html$ systemctl status nginx
● nginx.service - A high performance web server and a reverse proxy server
     Loaded: loaded (/lib/systemd/system/nginx.service; enabled; vendor preset: enabled)
     Active: active (running) since Sat 2024-10-12 06:32:37 UTC; 36s ago
       Docs: man:nginx(8)
   Main PID: 10215 (nginx)
      Tasks: 5 (limit: 9409)
     Memory: 4.9M
        CPU: 73ms
     CGroup: /system.slice/nginx.service
             ├─10215 "nginx: master process /usr/sbin/nginx -g daemon on; master_process on;"
             ├─10225 "nginx: worker process" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" ""
             ├─10226 "nginx: worker process" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" ""
             ├─10228 "nginx: worker process" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" ""
             └─10229 "nginx: worker process" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" ""

Oct 12 06:32:37 hellocloudnew systemd[1]: Starting A high performance web server and a reverse proxy server...
Oct 12 06:32:37 hellocloudnew systemd[1]: Started A high performance web server and a reverse proxy server.
Oct 12 06:32:37 hellocloudnew systemd[1]: Reloading A high performance web server and a reverse proxy server...
Oct 12 06:32:37 hellocloudnew systemd[1]: Reloaded A high performance web server and a reverse proxy server. -->


sudo systemctl stop nginx
sudo systemctl start nginx
sudo systemctl restart nginx
sudo systemctl reload nginx
sudo systemctl disable nginx
sudo systemctl enable nginx


#  set up a local DNS entry  for Testing Internal Applications

vi /etc/hosts
192.168.56.51 nginx.testing.com

 nslookup nginx.testing.com
Server:		127.0.0.53
Address:	127.0.0.53#53

Name:	nginx.testing.com
Address: 192.168.56.51

# modified Nginx Server Configuration
cd /etc/nginx/sites-available/

sudo nginx -t

sudo systemctl reload nginx
sudo ufw status

curl -I http://nginx.testing.com/




# Install Certbort
sudo snap install --classic certbot
cerbot --help

sudo ln -s /snap/bin/certbot /usr/bin/certbot


vagrant@hellocloudnew:/$ ls /etc/nginx/sites-available
default
vagrant@hellocloudnew:/$ ls /var/www/html
index.nginx-debian.html
vagrant@hellocloudnew:/$ 


# Install Laravel 
php -v
sudo apt install php
sudo apt install php-mbstring php-xml php-bcmath php-curl
sudo apt install php8.1-curl
sudo apt install php8.1-zip


# Instal composet
vagrant@hellocloudnew:/$ sudo php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
vagrant@hellocloudnew:/$ sudo php -r "if (hash_file('sha384', 'composer-setup.php') === 'dac665fdc30fdd8ec78b38b9800061b4150413ff2e3b6f88543c636f7cd84f6db9189d43a81e5503cda447da73c7e5b6') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
Installer verified
vagrant@hellocloudnew:/$ sudo php composer-setup.php
All settings correct for using Composer
Downloading...

Composer (version 2.8.1) successfully installed to: //composer.phar
Use it: php composer.phar

vagrant@hellocloudnew:/$ sudo php -r "unlink('composer-setup.php');"
vagrant@hellocloudnew:/$ sudo mv composer.phar /usr/local/bin/composer

vagrant@hellocloudnew:/$ composer --version
Composer version 2.8.1 2024-10-04 11:31:01
PHP version 8.1.2-1ubuntu2.19 (/usr/bin/php8.1)
Run the "diagnose" command to get more detailed diagnostics output.
vagrant@hellocloudnew:/$ 

cd ~
composer create-project --prefer-dist laravel/laravel travel_list

cd travel_list
php artisan

sudo mv ~/travel_list /var/www/travel_list
sudo chown -R www-data.www-data /var/www/travel_list/storage
sudo chown -R www-data.www-data /var/www/travel_list/bootstrap/cache

sudo nano /etc/nginx/sites-available/travel_list
nginx -t
sudo systemctl reload nginx

sudo ln -s /etc/nginx/sites-available/travel_list /etc/nginx/sites-enable

niginx -s reload



scp filename username@ip:/home/dell

scp username@ip:/home/ubuntu/filename ./
vi ~/.ssh/config
Host name(prod-srv or dev-srv)
  Hostname ip
  user name

curl ifconfig.me


cd ~/.ssh
nano config
chmod 600 ~/.ssh/config
Host targaryen
    HostName 192.168.1.10
    User daenerys
    Port 7654
    IdentityFile ~/.ssh/targaryen.key

Host tyrell
    HostName 192.168.10.20

Host martell
    HostName 192.168.10.50

Host *ell
    user oberyn

Host * !martell
    LogLevel INFO

Host *
    User root
    Compression yes
ssh john@dev.example.com -p 2322
Copy
To connect to the server using the same options as provided in the command above, simply by typing ssh dev, put the following lines to your "~/.ssh/config file:

~/.ssh/config
Host dev
    HostName dev.example.com
    User john
    Port 2322
--------------------------------------------------------------------------------
----------------------------------------------------------------------------------


# Loadbalancing
sudo apt install nodejs
mkdir service1
vi app.js

const { createServer } = require('http');

const hostname = '0.0.0.0';
const port = 3000;

const server = createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Service1:Hello World');
});

server.listen(port, hostname, () => {
  console.log(`Server running at http://${hostname}:${port}/`);
});

node app1.js

mkdir service2
vi app2.js

const { createServer } = require('http');

const hostname = '0.0.0.0';
const port = 3001;

const server = createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Service2:Hello World');
});

server.listen(port, hostname, () => {
  console.log(`Server running at http://${hostname}:${port}/`);
});

https://docs.nginx.com/nginx/admin-guide/load-balancer/

http {
    upstream backend {
        server backend1.example.com weight=5;
        server backend2.example.com;
        server 192.0.0.1 backup;
    }
}

server {
    location / {
        proxy_pass http://backend;
    }
}
-----------------------------------------------------
http {
    upstream lb {
        server localhost:3000;
        server localhost:3001;

    }
}

server {
    listen 80;
    server_name _;
     
    location /lb {
        proxy_pass http://lb;
    }
}

sudo nano /etc/nginx/nginx.conf
Add the following inside the http block in nginx.conf (or create an http block if it doesn't exist):

nginx
Copy code
http {
    upstream lb {
        server localhost:3000;
        server localhost:3001;
    }

    # Other http-level directives can go here



/etc/nginx/sites-available
nano /etc/nginx/sites-avaiable/lb

nginx -t
nginx -s reload

vagrant@hellocloudnew:/var/log/nginx$ cat /var/log/nginx/error.log
2024/10/15 08:14:07 [notice] 14654#14654: signal process started
2024/10/15 08:18:53 [emerg] 15132#15132: "http" directive is not allowed here in /etc/nginx/sites-enabled/lb:1
vagrant@hellocloudnew:/var/log/nginx$ sudo nginx -t
nginx: [emerg] "http" directive is not allowed here in /etc/nginx/sites-enabled/lb:1
nginx: configuration file /etc/nginx/nginx.conf test failed
vagrant@hellocloudnew:/var/log/nginx$ 


# off directly to port


------------------------------------------------------------------------------------------------------
--------------------------------------------------------------------------------------------------------


# Run with docker container

# Update the package list
sudo apt update

# Install Node.js (this will include npm as well)
sudo apt install nodejs npm
node -v  # Check Node.js version
npm -v   # Check npm version
npm init -y

# docker build
docker build -t app1-image .

# push image to dokcerhub
docker login

docker tag <local-image-name>:<tag> <dockerhub-username>/<repository-name>:<tag>
docker tag app1-image:latest aungmyohein/app1-image:latest


 docker login
Authenticating with existing credentials...
WARNING! Your password will be stored unencrypted in /home/vagrant/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credential-stores

Login Succeeded
vagrant@hellocloudnew:/$ docker tag app1-image:latest aungmyohein/app1-image:latest
vagrant@hellocloudnew:/$ docker push aungmyohein/app1-image:latest
The push refers to repository [docker.io/aungmyohein/app1-image]
dbccf1ef2b71: Pushed 
dc0b28b94bf1: Pushed 
3aaf20fa0338: Pushed 
f1457a4948b2: Pushed 
69bd3a0bcbca: Mounted from library/node 
9c2e3cad5dd9: Mounted from library/node 
46468d86f161: Mounted from library/node 
f48867d95974: Mounted from library/node 
2bce433c3a29: Mounted from library/node 
f91dc7a486d9: Mounted from library/node 
3e14a6961052: Mounted from library/node 
d50132f2fe78: Mounted from library/node 
latest: digest: sha256:fa6879c9a48772d47c7a8019f780e3e8eb849be95162ec289429ab8e83fc7454 size: 2832


# docker images
vagrant@hellocloudnew:/$ docker images
REPOSITORY               TAG            IMAGE ID       CREATED              SIZE
app2-image               latest         ed42080751e4   About a minute ago   1.09GB
aungmyohein/app1-image   latest         68afb27b631d   10 minutes ago       1.09GB
aungmyohein/app2-image   latest         68afb27b631d   10 minutes ago       1.09GB
app1-image               latest         68afb27b631d   10 minutes ago       1.09GB


#check port command
sudo netstat -tuln | grep LISTEN
sudo ss -tuln | grep LISTEN
lsof -iTCP -sTCP:LISTEN -n -P


# Create and Run Docker Container

# Run app1-container
docker run -d --name app1-container -p 3000:3000 app1-image

# Run app2-container
docker run -d --name app2-container -p 3001:3001 app2-image

vagrant@hellocloudnew:~/Tech-Lab/service2$ docker ps
CONTAINER ID   IMAGE                         COMMAND                  CREATED          STATUS          PORTS                                                                                      NAMES
951a1adcd216   app2-image                    "docker-entrypoint.s…"   15 seconds ago   Up 15 seconds   0.0.0.0:3001->3001/tcp, :::3001->3001/tcp                                                  app2-container
06511be8834a   app1-image                    "docker-entrypoint.s…"   16 seconds ago   Up 15 seconds   0.0.0.0:3000->3000/tcp, :::3000->3000/tcp 



