# Easy Wordpress

**Easy Wordpress** easily creates wordpress hosting environment.

**Easy Wordpress** is a shell script that rapidly installs wordpress on Ubuntu 12.04, by eliminating a lot of the up front setup.

- You don't need to install/configure LEMP Stack(Linux,nginx,MySQL,PHP). :)
- If you have already installed LEMP Stack, then it directly installs Wordpress on it.
- In a few minutes you'll be set up with a minimal, wordpress environment.
- You can easily remove Easy Wordpress Installation by running `cleanup` script.



##Pre-requisites

1. This script requires non-root user account on your server with sudo privileges for its execution or it should be executed with root user.
2. This script requires `sample_nginx_config` file for its execution.



##How to Set it up ?

    git clone https://github.com/joshisumit/easy_wordpress.git
    
Execute script with:

    bash easy_wordpress
    
    
## Verify your wordpress installation

Once the script has completed successfully,just open example.com in your browser, famous wordpress installation wizard will greet you (e.g. `example.com/wp-admin/install.php` ).

Verify following steps:

1. Check your example.com configuration (nginx server block) - `/etc/nginx/sites-available/example.com`
2. Check your example.com root directory - `/usr/share/nginx/www/example.com/htdocs`
3. Login to mysql database 


Check for example.com_db :
    
    show databases;
    
List all tables of example_db (i.e. tables starting with wp_) :    
    
    use example.com_db;
    show tables;
    
 Check example.com_db user(i.e. wordpuser) by runnning:
 
     SELECT User FROM mysql.user;


##Issues

#### Issue 1)  `Your PHP installation appears to be missing the MySQL extension which is required by WordPress`

Edit the following file:

    sudo nano /etc/php5/fpm/php.ini
    
Uncomment the following line, if it is commented and restart php5-fpm service

    ;extension=msql.so

#### Issue2) `nginx 502 - bad gateway`

Open the following file:

    sudo nano /etc/php5/fpm/pool.d/www.conf 

edit the line 

    listen = /var/run/php5-fpm.sock
    
Change it to:

    listen = 127.0.0.1:9000
    
 
