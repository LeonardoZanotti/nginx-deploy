<img src="https://www.nginx.com/wp-content/uploads/2018/08/NGINX-logo-rgb-large.png" alt="Nginx symbol">

# Nginx-deploy
Relatório que ensina a realizar o deploy de um projeto utilizando nginx <br />

How to do project deploy with Nginx.

## Requirements
Im using only Nginx 1.14.0. In distros from Debian it can be installed with:
```bash
$ sudo apt-get update
$ sudo apt-get install nginx
```
If you have some problem installing Nginx you can visit the [oficial page](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/).

## Other requirements
To host a project you obviously need some project. For trainning you can use the `html_test` project in this repository. It was just a random sample with html + css + js.
To get it you can just download this repo or clone it with:
```bash
$ git clone https://github.com/LeonardoZanotti/nginx-deploy.git
```
## Creating the virtual host
First of all, drop the project folder inside **/var/www/html** (you will need root acess) and garant some permissions to the project folder:
```bash
# Moving the project inside the folder
$ sudo mv /home/user/path/to/project /var/www/html

# Giving permissions
$ sudo chmod 755 /var/www/html/html_test
```
## Nginx configuration
Now, lets configure the host:
```bash
# Go to the nginx folder
$ cd /etc/nginx
```
Here we have this files:
```bash
conf.d          koi-win            nginx.conf       sites-enabled
fastcgi.conf    mime.types         proxy_params     snippets
fastcgi_params  modules-available  scgi_params      uwsgi_params
koi-utf         modules-enabled    sites-available  win-utf
```
Then, go to sites-enabled/
```bash
# Entering the folder
$ cd sites-enabled/
```
Inside sites-enabled we have just a `default` file, you can delete this. Now, lets create our conf file:
```bash
# Creating the file and openning with text editor
$ sudo nano html_test.conf
```
Configure html_test how you want. In my case:
```bash
server {
	listen 80;
	listen [::]:80;
	
	# Folder of the project
	root /var/www/html/html_test;

	# Names of main file to search
	index index.html index.htm;

	# URL of the virtual host
	server_name htmltestzanotti.com;

	location / {
		try_files $uri $uri/ =404;
	}
}
```
Now, just set the virtual host file:
```bash
$ sudo nano /etc/hosts

# add the following line in the file with your URL
127.0.0.1	htmltestzanotti.com
```

And restart the nginx service with:
```bash
$ sudo systemctl restart nginx
```

Its done! Just go to localhost or htmltestzanotti.com (your URL) to access your virtual host.

# LeonardoZanotti