![image](https://github.com/heluicare/some-note/raw/master/images/logo.jpg)
Once in a while you want to have a test web development environment in which you can test your ideas more freely. Considering that most server systems we have dealt with are all linux (I don’t know about you) setting up one on a windows system without using the existing WEMP( Windows Nginx Mysql Php) packages was a bit challenging for us. We then wrote this article to for us to have a place to jump to next time we need that. If somebody else finds it helpful even better!

We assume you have a mysql environment ready. You can easily download and install MYSQL for windows at their website.Now get to work:

Downoad Ngnix
Grab yourself a copy of the latest version of nginx at their website and extract it to a directory on your PC

![image](https://github.com/heluicare/some-note/raw/master/images/nginxinstall.png)

Mine is installed in :

C:\nginx_php\nginx-1.13.0
Download PHP
Grab a copy of PHP at their website as well and put it inside a “php” directory in where you put nginx. You can see the php directory in the image below.

![image](https://github.com/heluicare/some-note/raw/master/images/phpinstall.png)

Download the visual studio dependencies:
At the php download page in the sidebar there is critical info you should know. I quote them here :

More recent versions of PHP are built with VC11, VC14 or VC15 (Visual Studio 2012, 2015 or 2017 compiler respectively) and include improvements in performance and stability.

– The VC11 builds require to have the Visual C++ Redistributable for Visual Studio 2012 x86 or x64 installed

– The VC14 builds require to have the Visual C++ Redistributable for Visual Studio 2015 x86 or x64 installed

– The VC15 builds require to have the Visual C++ Redistributable for Visual Studio 2017 x64 or x86 installed

 

 

Basicaly, depending on the php binary you downloaded, you will need a different visual studio redistributable dependencies. A rather mechanic way to test this is to just run php as shown later in the guide and let the system tell you what it needs an example error is shown below for reference

Missing VCRUNTIME140.dll
This suggests that wee need to install the vs2015 redistribuabutables in this particular case, you can find a good discussion on the subject here. With all the downloads taken care of we can now configure our servers.

Configure nginx
Now we have all the files we need, time for some configuration, we start with nginx, open its configuration file located at

/conf/nginx.conf
in the nginx directory, the full path for my set up is :

C:\nginx_php\nginx-1.13.0\conf\nginx.conf
Open the file and uncomment (remove the #) the lines below :

\#location ~ \.php$ {

\#    root           html;

\#    fastcgi_pass   127.0.0.1:9000;

\#    fastcgi_index  index.php;

\#    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;

\#    include        fastcgi_params;

\#}

next edit fastcgi_param to point to the html directory inside nginx, this directory is where your webpage files will be hosted.My configuration is shown below , adjust according to your needs.

fastcgi_param  SCRIPT_FILENAME  C:/nginx_php/nginx-1.13.0/html$fastcgi_script_name;
Configure php
Head into the php directory and rename php.ini-production or php.ini-development to php.ini (depending on your needs). Next open the php.ini file inside the php directory and configure the path to the extension binaries. Mine is configured as shown below :

extension_dir = C:\nginx_php\nginx-1.13.0\php\ext
One last thing, because we use the mysql database, we need to enable the mysql drivers for them to be usable, do this in the same php.ini file as shown below , enable by removing the “;” :

extension=php_mysqli.dll
;extension=php_oci8_12c.dll  ; Use with Oracle Database 12c Instant Client
;extension=php_openssl.dll
;extension=php_pdo_firebird.dll
extension=php_pdo_mysql.dll
With all this, we are done with the basic configurations.

Start php
Go into the php directory. Hold SHIFT , right click on an empty spot and choose Open command window here, in the command line interface that shows up type the command :

php-cgi -b 127.0.0.1:9000

and press enter. PHP should now be up and running.

Start nginx
Head into the nginx directory and repeat the same steps, Hold SHIFT , right click on an empty spot and choose Open command window here,in the command line interface that shows up , type the command

nginx
and press enter. Your nginx instance should now be up and running.

Test it all
To test your setup just open your favorite browser and type

localhost
in the browser window and you should see a default nginx page confirming that your server is now working

![image](https://github.com/heluicare/some-note/raw/master/images/nginxtest.png)

Your web directory
Our nginx server is now configured to use the

C:\nginx_php\nginx-1.13.0\html
directory as the location for our web pages, if you go in there, you will find an index.html file that is generating the nginx welcome page we just saw above. This directory is configured in the nginx.conf file by the line :

root           html;
Create a new file in there and name it test.php and put in the content below :

<?php phpinfo(); ?>
Save the file and in your browser, visit

localhost/test.php
and you should see a page showing all the configurations for your php instance as shown below:

![image](https://github.com/heluicare/some-note/raw/master/images/phpinfo.png)

Now you have a working local server environment. I hope this has been informative for you and thanks for reading.


>from https://www.blikoontech.com/tutorials/how-to-install-nginx-and-php-on-windows

