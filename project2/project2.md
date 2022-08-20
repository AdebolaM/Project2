# This is my documentation for project 2.
## Installation of NGINX 
I updated 'apt' on the ubuntu with the

 ``` sudo apt update``` 

command then installed NGINX with 

```sudo apt install nginx```

I made sure that my server allows traffic from port 80 by updating the inbound rules on my EC2 instance security,

I can then access my server from Ubuntu through

```curl http://127.0.0.1:80```
![install nginx](https://github.com/AdebolaM/Project2/blob/main/project2/images/NGINX.png?raw=true)



## Installing mysql 
```$ sudo apt install mysql-server```
![istallmysql](https://github.com/AdebolaM/Project2/blob/main/project2/images/install%20mysql.png?raw=true)


I secured mysql configuation by setting the right password, removing test users and disallowing remote users

![securingmysql](https://github.com/AdebolaM/Project2/blob/main/project2/images/secured%20sql.png?raw=true)



## Installing php 

```sudo apt install php-fpm php-mysql```

![installphp](https://github.com/AdebolaM/Project2/blob/main/project2/images/install%20php.png?raw=true)


## Configuring php to use nginx 


This was exaltly like configuring apache in the provious project, but in this case nano text editor was used instead of VIM. They perform the same function. they are both text editors


I created a directory to contian the information for the confiuration 
```sudo mkdir /var/www/projectLEMP```

then I assigned the ownership of the dirctory to the user 

```sudo chown -R $USER:$USER /var/www/projectLEMP```

 then I opened a configure file using the nano command and install my configuration note or message

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

![syntax okay](https://github.com/AdebolaM/Project2/blob/main/project2/images/syntax%20congi.okay.png?raw=true)


 finally I disabled the dafault site in order for the new one to work 

 ```sudo unlink /etc/nginx/sites-enabled/default```


  and reloaded nginx 
 
![serveronunbuntu](https://github.com/AdebolaM/Project2/blob/main/project2/images/server%20visible%20on%20Ubuntu.png?raw=true)



![server visible on the browser](https://github.com/AdebolaM/Project2/blob/main/project2/images/working%20server.png?raw=true)

  I placed  a message in the php file so that i can show on the website

  ```sudo echo 'Hello LEMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectLEMP/index.html```




I proceeded to check this by checking the website in a broswer with  http//public IP:80


![phpconfig](https://github.com/AdebolaM/Project2/blob/main/project2/images/php%20configured%20website%20.png?raw=true)





##  Testin php with nginx

I created a test php configuration file in the root directory using the nano editor again


 ```sudo nano /var/www/projectLEMP/info.php```
 

 *<?php

phpinfo();


and then confirmed the configuration by checking the website with 
http //public IP/info.php


![phpfile](https://github.com/AdebolaM/Project2/blob/main/project2/images/php%20working%20fine%20.png?raw=true)


its important to delete the file created as it contains sensitive infomation about your server and php environment

```sudo rmdir /var/www/projectLEMP/info.php```


##  Retrieving data from mysql with php 

I created a database 'example_database' and a todo_list table in the database and proceeded to send through php to the website.

I created a user 'example_user'  with full privilages on the database 

```mysql> CREATE DATABASE `example_database`;```

```mysql> GRANT ALL ON example_database.* TO 'example_user'@'%';```

![createddb](https://github.com/AdebolaM/Project2/blob/main/project2/images/creating%20a%20DB.png?raw=true)


upon signing in with my new user, I confirmed that it has access by checking the databases available 

```mysql> SHOW DATABASES;```

![db](https://github.com/AdebolaM/Project2/blob/main/project2/images/DATABASE.png?raw=true)



I then created a table 'todo_list' using the following syntax one after the other ignoring the mysql at the begining of the each statement 

I didnt realise that I have to do that. *laughs* 
 

I also will be able to close the syntax with the ');'

"CREATE TABLE example_database.todo_list (
mysql>     item_id INT AUTO_INCREMENT,
mysql>     content VARCHAR(255),
mysql>     PRIMARY KEY(item_id)
mysql> );"






I Insert rows into the table by inserting the following command by changing the value of content every time 

```mysql> INSERT INTO example_database.todo_list (content) VALUES ("My first important item");```

checked my table with 
```mysql>  SELECT * FROM example_database.todo_list;```


![my table](https://github.com/AdebolaM/Project2/blob/main/project2/images/my%20table.png?raw=true)




I exited mysql and opened a new file in my root dicument with 

```nano /var/www/projectLEMP/todo_list.php```

and then placed the following infomation 


*<?php
$user = "example_user";
$password = "password";
$database = "example_database";
$table = "todo_list";

try {
  $db = new PDO("mysql:host=localhost;dbname=$database", $user, $password);
  echo "<h2*>TODO</h2><ol>";
  foreach($db->query("SELECT content FROM $table") as $row) {
    echo "<li>" . $row['content'] . "</li>";
  }
  echo "</ol>";
} catch (PDOException $e) {
    print "Error!: " . $e->getMessage() . "<br/>";
    die();
}


then reloaded the browser 


![todolistonline](https://github.com/AdebolaM/Project2/blob/main/project2/images/my%20todolist%20online.png?raw=true)





