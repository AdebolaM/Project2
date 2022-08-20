This is my documentation for project 2.
installation of NGINX 
i updated the 'apt' on the ubuntu with the

 ``` sudo apt update``` 

command then installed NGINX with 

```sudo apt install nginx```

making sure that my server allows trffic from port 80 by updating the inbound rules on my EC2 instance security,

i can access my server from Ubuntu through
```curl http://127.0.0.1:80```


# installing my sql 
```$ sudo apt install mysql-server```

i secured my sql configuation by setting the right password, removing test users and dis allowing remote users

installing php 
```sudo apt install php-fpm php-mysql```

configuring php to use nginx 


this was exaltly like configuring apache in th eprovious project, but in this case nano text editor was used instead of VIM. I guess they perform the same function. they are both text editors


i created a directory to contian the information for the confiuration 
```sudo mkdir /var/www/projectLEMP```
then i set assigned the ownership of the dirctory to the user 
```sudo chown -R $USER:$USER /var/www/projectLEMP```

 then i opened a configure file using the nano command and install my configuration note or message

 #``` #/etc/nginx/sites-available/projectLEMP

server {
    listen 80;
    server_name projectLEMP www.projectLEMP;
    root /var/www/projectLEMP;

index index.html index.htm index.php;

location / {
    #try_files $uri $uri/ =40 }

location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php8.1-fpm.sock; }
 location ~ /\.ht {
        deny all;
    }

} ```


then I confirmed the configuration by checking the sits available 

```sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/```
 and also confirm that the syntax is correct with 

 ```sudo nginx -t```

 finally I disabled the dafault site in order for the new one to work 

 ```sudo unlink /etc/nginx/sites-enabled/default```
  and reloaded nginx 
  
  I put in a message in the php file so that i can show on the website

  ```sudo echo 'Hello LEMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectLEMP/index.html```
