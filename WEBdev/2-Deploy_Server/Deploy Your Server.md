  <style>
  .TOC{
	  position: sticky;
	  top: 0;
	  left: 0;
  }
  </style>
  

1. [[#1 Connect and configure VPS|1 Connect and configure VPS]]
	1. [[#1 Connect and configure VPS#Connect by password|Connect by password]]
	1. [[#1 Connect and configure VPS#Configure user|Configure user]]
	1. [[#1 Connect and configure VPS#Install SWAP|Install SWAP]]
	1. [[#1 Connect and configure VPS#2 Set up SSH|2 Set up SSH]]
1. [[#Chuẩn bị các package cần thiết cho Ubuntu|Chuẩn bị các package cần thiết cho Ubuntu]]
1. [[#Tạo PostgreSQL database|Tạo PostgreSQL database]]
1. [[#Tạo python virtual eviroment|Tạo python virtual eviroment]]
	1. [[#Tạo python virtual eviroment#Tạo python virtual eviroment|Tạo python virtual eviroment]]
1. [[#Clone code and configuring project|Clone code and configuring project]]
	1. [[#Clone code and configuring project#Clone code|Clone code]]
	1. [[#Clone code and configuring project#Create new .env file|Create new .env file]]
		1. [[#Create new .env file#generate a new secret key|generate a new secret key]]
	1. [[#Clone code and configuring project#Migration|Migration]]
	1. [[#Clone code and configuring project#Collectstatic|Collectstatic]]
1. [[#Gunicorn|Gunicorn]]
	1. [[#Gunicorn#Testing Gunicorn’s Ability to Serve the Project|Testing Gunicorn’s Ability to Serve the Project]]
	1. [[#Gunicorn#Creating systemd Socket and Service Files for Gunicorn|Creating systemd Socket and Service Files for Gunicorn]]
	1. [[#Gunicorn#Checking for the Gunicorn Socket File|Checking for the Gunicorn Socket File]]
	1. [[#Gunicorn#Testing Socket Activation|Testing Socket Activation]]
1. [[#NGINX|NGINX]]
	1. [[#NGINX#Explain|Explain]]
	1. [[#NGINX#Set up NGINX|Set up NGINX]]





[/toc/] 
{{toc}} 
[[__TOC__]] 
[toc]

![]()

<https://www.digitalocean.com/community/tutorials/how-to-set-up-django-with-postgres-nginx-and-gunicorn-on-ubuntu-20-04>
#  1 Connect and configure VPS

## Connect by password

`ssh [username]@[serverhost]`  để ssh đến remote server ví dụ:<br>

```bash
ssh root@139.162.157.111
```
connect by ssh key
```
ssh [user]@[ip_addr] -i ~/.ssh/[ key_name]
```
## Configure user
tạo user `trongnhan`

```bash
sudo useradd --create-home --shell /bin/bash --groups sudo trongnhan
```

hoặc

```bash
sudo useradd -m -s /bin/bash -G sudo trongnhan
```

Change user to nhan ``` su trongnhan ``` Change user to root ``` su -i ``` 

```bash
	passwd nhan
# Đổi password cho nhan user
cat /etc/passwd
#check xem user dc tạo chưa
groups nhan
# check group
su nhan # chuyen sang user nhan
su - # chuyen sang user root

```

Đổi host name trên VPS 
```bash
sudo hostnamectl set-hostname < server_name>
```
##  Install SWAP


```bash
sudo fallocate -l 1G /swapfile                                 # allocate /swapfile folder  
sudo dd if=/dev/zero of=/swapfile bs=1024 count=1048576        # wipe this file out
```
We need block size to create the swap file, block size is the size of swap file in mb multiplied by 1024. 
So if we are creating 1gb or 1024 mb swap file, the block size would be 1024 multiplied by 1024, which is equal to 1048576

```
sudo chmod 600 /swapfile                                       # readable only by root  
sudo mkswap /swapfile                                          # make swap file  
sudo swapon /swapfile                                          # turn on swap file
sudo nano /etc/fstab 
```
Thêm dòng này vào file `/etc/fstab` 
`/swapfile swap swap defaults 0 0`


## 2 Set up SSH
Check this page[[Set up SSH]]

coppy ssh pub key to remote server
```bash
ssh-copy-id -i ~/.ssh/Public_key.pub  nhan@[ip_addr] 
```

check ip addr của server
`ip addr `
Là cái inet của eth0

add config file `nano ~/.ssh/config`
```yaml
Host linode_NoobMaster
	Hostname [ip_addr]
	User nhan
	IdentityFile ~/.ssh/ssh_key_name
```



#  Chuẩn bị các package cần thiết cho Ubuntu
Install Nala nếu có thể: <https://christitus.com/stop-using-apt/>


```bash
sudo apt update && sudo apt upgrade -y
sudo apt install python3-venv python3-dev libpq-dev postgresql postgresql-contrib nginx curl
```
# Tạo PostgreSQL database
>By default, Postgres uses an authentication scheme called “peer authentication” for local connections. Basically, this means that if the user’s operating system username matches a valid Postgres username, that user can login with no further authentication.
>During the Postgres installation, an operating system user named postgres was created to correspond to the postgres PostgreSQL administrative user. You need to use this user to perform administrative tasks. You can use sudo and pass in the username with the -u option

psql is a terminal-based front-end to PostgreSQL
```bash
sudo -u postgres psql
```

Tạo database cho project
```sql
CREATE DATABASE DBnoob;
```
```sql
select datname from pg_database; -- check if DB exist
```

Tạo role cho DB mới tạo
```sql
CREATE ROLE admindbnoob;
```
```sql
SELECT * FROM pg_catalog.pg_roles WHERE  rolname = 'admindbnoob'; --kiểm tra xem role dc tạo chưa
```

Gán quyền admin của DB cho ROLE vừa tạo
```sql
GRANT ALL PRIVILEGES ON DATABASE dbnoob TO noobmaster;
```

Next, create a database user for our project. Make sure to select a secure password:
```sql
CREATE USER username WITH PASSWORD 'pass';
```
```sql
select usename from pg_catalog.pg_user; --kiểm tra xem user dc tạo chưa
```
```sql
ALTER USER user_name WITH PASSWORD 'new_password';
```




Afterwards, you’ll modify a few of the connection parameters for the user that you just created. This will speed up database operations so that the correct values do not have to be queried and set each time a connection is established.
You will set the default character encoding to UTF-8, which Django expects. You are also setting the default transaction isolation scheme to “read committed”, which blocks reads from uncommitted transactions. Lastly, you are setting the timezone. By default, Django projects will be set to use UTC.

```sql
ALTER ROLE noobmaster SET client_encoding TO 'utf8';
```
```sql
ALTER ROLE noobmaster SET default_transaction_isolation TO 'read committed';
```
```sql
ALTER ROLE noobmaster SET timezone TO 'UTC';
```


exit out of the PostgreSQL prompt by typing
```sql
\q
```


#  Tạo python virtual eviroment 
## Tạo python virtual eviroment
Tải a specific python version
```bash
sudo add-apt-repository ppa:deadsnakes/ppa #Deadsnakes is **a truly wonderful repository containing builds of alternative Python versions**
```
```bash
sudo apt install python3.6
```
Có các cách tạo sau
- Khi ta tạo python virtualenv dùng luôn python version chính của OS: <https://www.digitalocean.com/community/tutorials/how-to-set-up-django-with-postgres-nginx-and-gunicorn-on-ubuntu-22-04#creating-a-python-virtual-environment-for-your-project>
-  Tạo virtual enviroment với một python version khác: 
```bash
python -m virtualenv --python="/usr/bin/python3.6" "/path/to/new/virtualenv/"
```
```bash
source venv/bin/activate
```
- (Miniconda) Tạo virtual enviroment với một python version khác một cách bạo lực: <https://waylonwalker.com/install-miniconda/> 


# Clone code and configuring project
## Clone code
Copy to server
	`scp -r localmachine/path_to_the_directory/* username@server_ip:/path_to_remote_dir`
Unzip file
`unzip file.zip`

## Create new .env file
Copy .env file to server
`scp -r localmachine/path_to_the_directory/* username@server_ip:/path_to_remote_dir`


### generate a new secret key
```bash
python manage.py shell
>> from django.core.management.utils import get_random_secret_key
>> x = get_random_secret_key()
>> x
```


## Migration
_**"migrate" with "--run-syncdb" creates tables for apps without migrations.**_
```bash
python manage.py migrate --run-syncdb 
```
## Create super user 

`python manage.py createsuperuser`

## Collectstatic
đầu tiên phải uncomment *'django.contrib.staticfiles'* trong *INSTALLED_APP*

```bash
python manage.py collectstatic
```
Sau đó comment *'django.contrib.staticfiles'* trong *INSTALLED_APP* lại




# Gunicorn
If you followed the initial server setup guide, you should have a UFW firewall protecting your server. In order to test the development server, we’ll have to allow access to the port we’ll be using.

Create an exception for port 8000 by typing:
```bash
sudo ufw allow 8000
```
```bash
python manage.py runserver 0.0.0.0:8000
```

## Testing Gunicorn’s Ability to Serve the Project

```bash
(virtualenv) nhan@linode-NoobMaster:~/nhan_TheNoobMaster/src_Django$ gunicorn --bind 0.0.0.0:8000 nhan_TheNoobMaster.wsgi
```
We passed Gunicorn a module by specifying the relative (to the folder we run this command) directory path to Django’s `wsgi.py` file, which is the entry point to our application, using Python’s module syntax.

Exiting environment
```bash
deactivate
```

## Creating systemd Socket and Service Files for Gunicorn
e have tested that Gunicorn can interact with our Django application, but we should implement a more robust way of starting and stopping the application server. To accomplish this, we’ll make systemd service and socket files.

The Gunicorn socket will be created at boot and will listen for connections. When a connection occurs, systemd will automatically start the Gunicorn process to handle the connection.

Start by creating and opening a systemd socket file for Gunicorn with `sudo` privileges:
```bash
sudo vim /etc/systemd/system/gunicorn.socket
```

/etc/systemd/system/gunicorn.socket
```
[Unit]
Description=gunicorn socket

[Socket]
ListenStream=/run/gunicorn.sock

[Install]
WantedBy=sockets.target
```

Next, create and open a systemd service file for Gunicorn with `sudo` privileges in your text editor. The service filename should match the socket filename with the exception of the extension:

```bash
sudo vim /etc/systemd/system/gunicorn.service
```

/etc/systemd/system/gunicorn.service
```
[Unit]
Description=gunicorn daemon
Requires=gunicorn.socket
After=network.target


[Service]
User=sammy
Group=www-data
WorkingDirectory=/home/ <user_name> / <project_dir>
ExecStart=/home/ <user_name> / <project_dir> / <project_env> /bin/gunicorn \
          --access-logfile - \
          --workers 3 \
          --bind unix:/run/gunicorn.sock \
          <project_name>.wsgi:application
          

[Install]
WantedBy=multi-user.target
```

We can now start and enable the Gunicorn socket. This will create the socket file at `/run/gunicorn.sock` now and at boot. When a connection is made to that socket, systemd will automatically start the `gunicorn.service` to handle it:
```bash
sudo systemctl start gunicorn.socket
```
```bash
sudo systemctl enable gunicorn.socket
```

## Checking for the Gunicorn Socket File
Check the status of the process to find out whether it was able to start:

```bash
sudo systemctl status gunicorn.socket
```

Copy

You should receive an output like this:

```
Output● gunicorn.socket - gunicorn socket
     Loaded: loaded (/etc/systemd/system/gunicorn.socket; enabled; vendor prese>
     Active: active (listening) since Fri 2020-06-26 17:53:10 UTC; 14s ago
   Triggers: ● gunicorn.service
     Listen: /run/gunicorn.sock (Stream)
      Tasks: 0 (limit: 1137)
     Memory: 0B
     CGroup: /system.slice/gunicorn.socket
```

Next, check for the existence of the `gunicorn.sock` file within the `/run` directory:

```bash
file /run/gunicorn.sock
```

```
Output/run/gunicorn.sock: socket
```

If the `systemctl status` command indicated that an error occurred or if you do not find the `gunicorn.sock` file in the directory, it’s an indication that the Gunicorn socket was not able to be created correctly. Check the Gunicorn socket’s logs by typing:
```bash
sudo journalctl -u gunicorn.socket
```
Take another look at your `/etc/systemd/system/gunicorn.socket` file to fix any problems before continuing.

## Testing Socket Activation
Currently, if you’ve only started the `gunicorn.socket` unit, the `gunicorn.service` will not be active yet since the socket has not yet received any connections. You can check this by typing:
```bash
sudo systemctl status gunicorn
```
To test the socket activation mechanism, we can send a connection to the socket through `curl` by typing:
```bash
curl --unix-socket /run/gunicorn.sock localhost
```
You should receive the HTML output from your application in the terminal. This indicates that Gunicorn was started and was able to serve your Django application. You can verify that the Gunicorn service is running by typing:

```bash
sudo systemctl status gunicorn
```

# NGINX

## Explain
HTTP requests are handle by a web server, a specialize webserver Nginx in this case
Nginx know how to handle HTTPs while Django and Gunicorn can’t do this
Nginx is a webserver, a proxy and a load balancer

![simpliest Architecture](simpliestArchitecture.png "simpliest Architecture")



```
server {
    listen 80;
    server_name server_domain_or_IP;

    location = /favicon.ico { access_log off; log_not_found off; }
    location /static/ {
        root /home/<user_name>/ <myProjectDir>;
    }

    location / {
        proxy_pass http://localhost:8000;
    }
}
```

![](explain_NGINX.png)

##  Set up NGINX

- Install NGINX

To configure sofware behaviour we create Nginx config file inside `/etc` folder

`sudo nano /etc/nginx/sites-available/<mywebsite>`

```
server {
    listen 80;
    server_name server_domain_or_IP;

    location = /favicon.ico { access_log off; log_not_found off; }
    location /static/ {
        root /home/<user_name>/<myProjectDir>;
    }

    location / {
        include proxy_params;
        proxy_pass http://unix:/run/gunicorn.sock;
    }
}
```

By default NGINX does not serve file from  `/etc/nginx/sites-available/` instead it serve file from folder `/etc/nginx/sites-enabled`
Save and close the file when you are finished. Now, we can enable the file by linking it to the `/etc/nginx/sites-enabled` directory:

```
sudo ln -s /etc/nginx/sites-available/<project_name> /etc/nginx/sites-enabled

```

Test your Nginx configuration for syntax errors by typing:

```
sudo nginx -t

```

If no errors are reported, go ahead and restart Nginx by typing:

```
sudo systemctl restart nginx

```

Finally, we need to open up our firewall to normal traffic on port 80. Since we no longer need access to the development server, we can remove the rule to open port 8000 as well:

```
sudo ufw delete allow 8000
sudo ufw allow 'Nginx Full'

```

You should now be able to go to your server’s domain or IP address to view your application.


4665840138155814



