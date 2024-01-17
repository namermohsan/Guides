Deploying Flask to WSL Ubuntu Linux

1-	We need to enable the WSL feature on windows, we can do that by searching for ‘control panel’ (from the start menu) –
A-	 >then choose Programs 
B-	 >Programs and Features
C-	 > Turn Windows Features On or Off 
D-	 > Enable (Windows SubSystem For Linux) feature then click ok.
 
2- Download the WSL from Microsoft Store by searching for WSL and then download the Distribution you want to use (assuming Ubuntu is chosen).

If you want to download a different wsl Distribution from the CMD, run it as administrator and type: 


>wsl --install --distribution Debian

If you encounter an error trying to start the distribution, try running:


>wsl --update


(This command will update the WSL)

3- reboot your system

4- run wsl as administrator

Note: If you want to run linux as a Virtual Machine you need to enable the virtualization feature in windows ( by following steps explained in step number 1 then enabling virtualization feature, also enable Hyper V)

- To find available distributions online run:


>wsl --list online


5- run the WSL, set a username and a password:
-	If it doesn’t accept the password try run: 


><the name you chose> --force-badname


and give it a password.

6- to update the distribution ( assuming Ubuntu is installed) run :

>sudo apt update

then :

>sudo apt dist-upgrade -y

7- To access the WSL files from windows, simply type in in the address bar: \\wsl$\ 
then choose where do you want to put the files on the WSL

8- install apache server on WSL by running: 

>sudo apt install apache2 -y   (assuming Ubuntu is downloaded)

- install the wsgi_mod by running: 

>sudo -H apt install libapache2-mod-wsgi-py3

9- create a directory in the home directory by running: 

>mkdir flask_app

10- copy your flask application from windows to the directory 'flask_app' on the WSL

11- make a file in this working directory with the name app.wsgi that contains the configuration: 


import sys

sys.path.insert(0, '/var/www/html/flask_app')

from app import app as application


12- install the required python packages to run for our first application (Project1) by running:

>	sudo -H pip3 install flask   (if it doesn't work try running: sudo apt install pip)
>	sudo -H pip3 install flask_migrate
>	sudo -H pip3 install flask_sqlalchemy
>	sudo -H pip3 install pymssql

13- copy the flask application to the /var/www/html by running:

>	sudo cp -R flask_app/ /var/www/html/


 (assuming that the current working directory is the home directory)

14- modify the apache service configuration by running:

>	sudo nano /etc/apache2/sites-available/000-default.conf

 #DocumentRoot /var/www/html

        WSGIScriptAlias / /var/www/html/flask_app/app.wsgi
        <Directory /var/www/html/flask_app>
        Order allow,deny
        Allow from all
        </Directory>

15- navigate to the web flask application directory by running:

>	sudo cd /var/www/html/flask_app 

16- optionally you can switch to the root user or run the commands with sudo, to switch to root:

>	 sudo su - root

17- migrate the database:

>	export FLASK_APP=app.py
>	flask db init
>	flask db migrate -m 'migration'
>	flask db upgrade

18- restart the apache service by running: sudo service apache2 restart'


Note: If you encouter an installation issue with pymssql on windows try to remove it with #pip remove pymssql
and then install C++ Build Tools for Windows, or downgrade your pymssql to version 2.2.7 by running:# pip install pymssql==2.2.7

 
Wish you all the very best,

Nameer






