
# Connecting to AWS and creating a new EC2 instance

After setting up your account and selecting ubuntu as your preferred OS then you would proceed to creating a new instance, you would need to save your private key (.pem file). Once that's done you would need to create a new security group for your instance “ at least that's what i had to do before i was able to establish a connection between my terminal and AWS server” , connecting to AWS via terminal is done by copying the ssh
 Command on AWS. 
Go to your terminal
Type the command ‘ls’ and then locate the folder where your private key for AWS was saved
To open the folder type the command code ‘cd Downloads’ as that is where my private key was saved on my mac.
Once you are in the folder type the code ‘ls’ again and confirm you have your private key there by checking the list.
Change premissions for the private key file (.pem), otherwise you can get an error "Bad permissions"   sudo chmod 0400 <private-key-name>.pem  
Paste the ssh command from AWS to connect.  


IMPORTANT - Anywhere you see these anchor tags < > , going forward, it means you will need to replace the content in there with values specific to your situation. For example, if we need you to replace the name you have saved the private key on your machine, we will write something like < private-key-name > If the private key you downloaded was named my-private-key.pem simply remove the anchor tags and insert my-private-key.pem in the command you are required to execute.  ![YQTY4ludc6HycNVug6CRCZU1WtMWo6Fu3IYUofFDEBUYSdBa7EM7kjkxOL6xEiIEFQx04yxBd9w06Ez75dXFLgl8jmmMbmigvccxMC5CcoG4mTlwxF_nF1EW6TAD](https://user-images.githubusercontent.com/127052041/222983374-c7947b58-c84f-4b07-9238-1adcf7303741.png)

 ![NuUcLuEE6MVnOtKJZRpxhE9cgTnMnF2q1YA6h7apRg1Br4r8SpAK2u8jYkx2FUCyxmRPhFI5GPJXVRQPR9V_fEPnUTzZcVUJUXgk77rYTLX3GL2bQDBscdOB9mdy](https://user-images.githubusercontent.com/127052041/222983498-2032e9f8-de14-40b7-8781-5d5529e86807.png)
  
  #  Installing my first web server APACHE and updating the fire wall
We are creating an APACHE web server ‘HTTP’ on Ubuntu’s package manager. We are using Apache because it is among the most popular web servers in the world. It’s well documented, has an active community of users, and has been in wide use for much of the history of the web, which makes it a great default choice for hosting a website, it is also an open source available for free. 

To start we need to update update a list of packages in package manager, so run this command on your terminal sudo apt update
Now run apache2 package installation, use this command sudo apt install apache2
To verify that apache2 is running as a Service in our OS use the following command, if it is green and running then you did everything correctly  sudo systemctl status apache2 
That's it! My first web cloud server is up and running. 
<img width="770" alt="SLPmdwecPdY2CyOWVXDE5K7wM-904JH_3r3wEqrJ4pMaQGUfzXPtfjwu01-uwOBgEJWXWwL3YZvVFoecQXJZAX6kG7T8O5JsoDH32Uf-kcoYKRi8mm9VllMILLUq" src="https://user-images.githubusercontent.com/127052041/222983750-dee75ab5-621e-445e-b36d-8688f21ff09f.png">
<img width="535" alt="buntu@ip-172-31" src="https://user-images.githubusercontent.com/127052041/222983782-ea675392-ada5-459c-b85d-e6407bf4add8.png">
  ![FkUgtJl6WFsuF0_aGrBGezIDLMc8DJvPKShn_nV8lcBiIpb5CGpt7bfO1OnNYvxG5nlV3_LUZz6OnEDxyRlu4Botjrd-Qtvs5IuVOB9oGEHA2IV3EQrYT4lLI7yw](https://user-images.githubusercontent.com/127052041/222983802-fd528680-f752-4a43-9a72-b5b5c027fd9b.png)
  
   In other to start receiving traffic on my web server we need to open TCP 80 on our AWS instance inbound secuirty. TCP 80 is the default port web browsers use to access web pages on the Internet. As we know, we have TCP port 22 open by default on our EC2 machine to access it via SSH, so we need to add a rule to EC2 configuration to open inbound connection through port 80. 
![wzAXF3uxmIYKE5bdGpFeI1XEQCxqEfWfqPnR6yWNcuZTR3qFRriDruxGF01cQ3mrXj_XT-OcpV2dGiWltQHGR3s2UXXJEcZ1nWchmrwChVrIu3Jgw33rYKDFOGbq](https://user-images.githubusercontent.com/127052041/222983852-6cf07b25-db7b-4b70-98b5-00b897c01de0.png)

Once that's done lets access it locally in our Ubuntu shell by running this command curl http://localhost:80
or
 curl http://127.0.0.1:80
These 2 commands above actually do pretty much the same – they use ‘curl’ command to request our Apache HTTP Server on port 80 (actually you can even try to not specify any port – it will work anyway). The difference is that: in the first case we try to access our server via DNS name and in the second one – by IP address (in this case IP address 127.0.0.1 corresponds to DNS name ‘localhost’.
![73mFW8LkmF-rrChY5s3e0J7XUhjn4xGbeJNilDtC68yoBqnLxT75UfYKyY75SQ52Ake2T0SrD_ww93HsZFYvCdEkn7PetbevI-V9WFl0PMOYEncmAheVI-sOOxd7](https://user-images.githubusercontent.com/127052041/222983908-7fc88ab6-9edc-43ea-9e8c-999b28d849e7.png)
  That’s it! now let's test our APACHE HTTP server on any browser and see if we can get a response, open a browser and put your EC2 IP http://<Public-IP-Address>:80   
You can get your IP directly from your terminal instead of going to your AWS by using this code curl -s http://169.254.169.254/latest/meta-data/public-ipv4
  <img width="633" alt="gubEpku4JWEGLNvxGU4jz_RjXLTwzM11aFO-wiTL7PCy4jjjpqiJsVwyIH8NSgo1RDrpYibgakIsVFrqssxo7csrV9tnXvg9_368NVTsagM_CFx7cfvSzjBD3s_N" src="https://user-images.githubusercontent.com/127052041/222983952-cfd5697f-ed51-4274-9ca2-0e7cefff9a95.png">
![Ubuntu](https://user-images.githubusercontent.com/127052041/222983994-1edb32eb-0f2c-454e-a42f-54ff3f5cf877.png)

What you see here is the same content that you previously got by ‘curl’ command, but represented in nice HTML formatting by your web browser.

  
  # INSTALLING MYSQL   

  I have successfully installed my first web server, so now I need to create a Database Management System [DBMS] on my web server. MySQL is a popular relational database management system used within PHP environments so that's what we will be using. 
To install the software run this command $ sudo apt install mysql-server
Once the installation is complete run this command to login to MYSQL console, this will connect to the MySQL server as the administrative database user root, sudo mysql 
![9vfOtUN_XBY0hL7OVMqD_seNAAaGrFhKzxOfg8XDh7JjjBqeZu7xA-3xVyvNmJopA3RGRCdHVCcbbAWbp6J0jZy-WIjP5rukTRuyzaKfxgyZwF4SZr1tl7j4D2UE](https://user-images.githubusercontent.com/127052041/222984127-5ff35b1f-f4cd-4689-8565-7bd50b051e82.png)
  <img width="574" alt="Server version  0 32-@ubuntu0 22 04 2 (Ubuntu)" src="https://user-images.githubusercontent.com/127052041/222984177-86775072-bab9-472d-9b6f-f829bc9f7f1a.png">

    Now we need to set a password for the root user so run this command ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1'; 
You can use any preferred password of your choice by changing the 'PassWord.1' but for now let's leave it at that.
Exit the MySQL shell by running this command mysql> exit
Start the interactive script by running this command $ sudo mysql_secure_installation
   Note you will be asked to input your root password. You would see this afterwards       VALIDATE PASSWORD COMPONENT can be used to test passwords
and improve security. It checks the strength of password
and allows the users to set only those passwords which are
 secure enough. 
Select yes and select the kind of security you want required from your end user. 
Regardless of whether you chose to set up the VALIDATE PASSWORD PLUGIN  your server will next ask you to select and confirm a password for the MySQL root user. This is not to be confused with the system root. The database root user is an administrative user with full privileges over the database system. Even though the default authentication method for the MySQL root user dispenses the use of a password, even when one is set, you should define a strong password here as an additional safety measure. We’ll talk about this in a moment.
If you enabled password validation, you’ll be shown the password strength for the root password you just entered and your server will ask if you want to continue with that password. If you are happy with your current password, enter 
Y
 for “yes” at the prompt:
Estimated strength of the password: 100 
Do you wish to continue with the password provided?(Press y|Y for Yes, any other key for No) : y
For the rest of the questions, press 
Y
 and hit the 
ENTER
 key at each prompt. This will prompt you to change the root password, remove some anonymous users and the test database, disable remote root logins, and load these new rules so that MySQL immediately respects the changes you have made.
When you’re finished, test if you’re able to log in to the MySQL console by typing:
$ sudo mysql -p
Notice the 
-p
 flag in this command, which will prompt you for the password used after changing the root user password.
To exit the MySQL console, type:
mysql> exit
Notice that you need to provide a password to connect as the root user.
For increased security, it’s best to have dedicated user accounts with less expansive privileges set up for every database, especially if you plan on having multiple databases hosted on your server.
Note: At the time of this writing, the native MySQL PHP library 
mysqlnd
 doesn’t support 
caching_sha2_authentication
, the default authentication method for MySQL 8. For that reason, when creating database users for PHP applications on MySQL 8, you’ll need to make sure they’re configured to use 
mysql_native_password
 instead.
Your MySQL server is now installed and secured.
![us-kHMnWV1Oe-XvmVIeb8dlyvnhm--J8alKOhP7bWhWSgC9NomUHET9cfhrac3Fh_Z2FhCodUfcy3t4wou13S3nfq81ukN6Wx9eigP5zcqF5qKsXoEVWGVFlpOGR](https://user-images.githubusercontent.com/127052041/222984240-d4fa257c-dc76-4dad-b501-23ce0dbf9484.png)

  
  # INSTALLING PHP
  
  So I have successfully installed APACHE http to serve my content and MySQL installed to store and manage my data. PHP is the component of our setup that will process code to display dynamic content to the end user. In addition to the php package, you’ll need php-mysql, a PHP module that allows PHP to communicate with MySQL-based databases. You’ll also need libapache2-mod-php to enable Apache to handle PHP files. Core PHP packages will automatically be installed as dependencies.

To install these 3 packages at once, run this command sudo apt install php libapache2-mod-php php-mysql
Once the installation is finished, you can run the following command to confirm your PHP version: php -v
PHP 7.4.3 (cli) (built: Oct  6 2020 15:47:56) ( NTS )
Copyright (c) The PHP Group
Zend Engine v3.4.0, Copyright (c) Zend Technologies
At this point, your LAMP stack is completely installed and fully operational.
Linux (Ubuntu)
Apache HTTP Server
MySQL
PHP

  ![kAktXP86Zxm1m22lziB58lKIW30yhXC9v-W6NJHg1QqFwy8ul29xeRwMZpkVMDiEkwdEGDue44tMZQ4t7zn-_DMi2eoQoUnCi0P0oA5G7qNkZcjDs1Jd087eHFZE](https://user-images.githubusercontent.com/127052041/222984335-1667fb01-adce-493a-b2b2-267426df97bd.png)
  <img width="565" alt="-1ubuntu2 10 cli) (built Jan 16 2023 151949) (NTS)" src="https://user-images.githubusercontent.com/127052041/222984368-48527605-06a1-46dd-81af-d893015d5ed8.png">



  # CREATING A VIRTUAL HOST FOR MY WEBSITE USING APACHE

   Let's set up a domain name piusprojectlamp, you can use any name you like though.  Apache on Ubuntu 20.04 has one server block enabled by default that is configured to serve documents from the /var/www/html directory. We will leave this configuration as is and will add our own directory next to the default one.    
 Lets create a dirctory with the name piusprojectlamp using “mkdir” command sudo mkdir /var/www/piusprojectlamp 
Next, we assign ownership of the directory with your current system user by using this command  sudo chown -R $USER:$USER /var/www/piusprojectlamp
Next we create and open a new configuration file in Apache’s sites-available directory using your preferred command-line editor. Here, we’ll be using vi or vim (They are the same by the way)  sudo vi /etc/apache2/sites-available/piusprojectlamp.conf
 The previous command will create a blank file type on ‘i’ on your keyboard to insert the following configuration. Once inserted click on your esc button and then type ’:’ after that type ‘wq’ and then press your enter button to save and leave the page. <VirtualHost *:80>
    ServerName piusprojectlamp
    ServerAlias www.piusprojectlamp 
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/piusprojectlamp
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
Use the ls command to show the new file in the sites-available directory sudo ls /etc/apache2/sites-available 
 You will see something like this pop up 000-default.conf  default-ssl.conf  piusprojectlamp.conf
 Now let's use a2ensite command to enable our virtual host sudo a2ensite piusprojectlamp
Now lets disable the default website that comes installed with apache.  This is required if you’re not using a custom domain name, because in this case Apache’s default configuration would overwrite your virtual host. To disable Apache’s default website use a2dissite command sudo a2dissite 000-default
To make sure your configuration file doesn’t contain syntax errors run sudo apache2ctl configtest
Finally lets reload apache so these changes take effect  sudo systemctl reload apache2
          The website is live but the webroot /var/www/piusprojectlamp is empty Create an     index.html file in that location so that we can test that the virtual host works as expected: sudo echo 'Hello LAMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/piusprojectlamp/index.html
Now go to your browser and try to open your website URL using IP address of DNS 

If you see the text from ‘echo’ command you wrote to index.html file, then it means your Apache virtual host is working as expected.
In the output you will see your server’s public hostname (DNS name) and public IP address. You can also access your website in your browser by public DNS name, not only by IP – try it out, the result must be the same.

![Qnn4dtLdNOVjZsNoCpGHFTZBJRY7TbD3_NXwZ8OEKzGfIhjGSdH7iipwwyjB5CoNEJT35accdP68teWE_SWt3FazLIlSOU_GTsXpaK5x9lymWGKUz98xgq6NQIXK](https://user-images.githubusercontent.com/127052041/222984425-979ece31-426d-4f68-93e5-b5e68d604065.png)
![b7-IgEvylB-Zt6gCPKi9muJoGGrFdCE56GY9KF2Z67owMkiUja8A92q1mNjiWfYVRP_PX8Ssk_KqyaJyy-X_yln3WBiEhdhf6DkctPIiNGawr-TVK3-8vQN0VLH3](https://user-images.githubusercontent.com/127052041/222984447-cd1e07de-c4e9-4bbe-bb97-a149b07484cc.png)

  
  # ENABLE PHP ON THE WEBSITE
With the default DirectoryIndex settings on Apache, a file named index.html will always take precedence over an index.php file. This is useful for setting up maintenance pages in PHP applications, by creating a temporary index.html file containing an informative message to visitors. Because this page will take precedence over the index.php page, it will then become the landing page for the application. Once maintenance is over, the index.html is renamed or removed from the document root, bringing back the regular application page.
In case you want to change this behavior, you’ll need to edit the /etc/apache2/mods-enabled/dir.conf file and change the order in which the index.php file is listed within the DirectoryIndex directive: sudo vim /etc/apache2/mods-enabled/dir.conf
<IfModule mod_dir.c>
        #Change this:
        #DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
        #To this:
        DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>
Now that you have a custom location to host your website’s files and folders, we’ll create a PHP test script to confirm that Apache is able to handle and process requests for PHP files.
Create a new file named index.php inside your custom web root folder: sudo vim /var/www/piusprojectlamp/index.php
This will open a blank file. Add the following text, which is valid PHP code, inside the file: <?php 
                                                phpinfo();
Now check your website you should see information about your server from the perspective of PHP. It is useful for debugging and to ensure that your settings are being applied correctly.
If you can see this page in your browser, then your PHP installation is working as expected.
After checking the relevant information about your PHP server through that page, it’s best to remove the file you created as it contains sensitive information about your PHP environment and your Ubuntu server. You can use rm to do so: sudo rm /var/www/projectlamp/index.php
You can always recreate this page if you need to access the information again later.
<img width="729" alt="SBfKH9YLiUpWw3Xx8uVVtWlQVDv6B22NtM7TiHX2hkHRQSgaKjqOMt4NfEOOGCPoM1Y2uT888PRyVZQgCFRYk9wWA8M2JJZpeTEWGlHeVgwS9CQ_fnxu5ikGXWF6" src="https://user-images.githubusercontent.com/127052041/222984486-be797972-8f8c-4bde-b98e-dcbd837739d5.png">
<img width="486" alt="ubuntu@ip-172-31-30-6- vim varwwwprojectlampindex php" src="https://user-images.githubusercontent.com/127052041/222984523-e40a3b4f-2e57-4945-83da-1972cdd1ec4e.png">
![php](https://user-images.githubusercontent.com/127052041/222984546-75a4c170-8329-41ee-bcad-666da00755f7.png)
