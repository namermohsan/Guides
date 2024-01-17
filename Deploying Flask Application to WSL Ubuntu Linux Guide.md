# Deploying Flask to WSL Ubuntu Linux

1. **Enable WSL Feature on Windows:**
   - Search for 'control panel' from the start menu.
   - Choose Programs > Programs and Features > Turn Windows Features On or Off.
   - Enable (Windows SubSystem For Linux) feature then click ok.

2. **Download WSL from Microsoft Store:**
   - Search for WSL and download the Distribution you want to use (e.g., Ubuntu).
   - To download a different WSL Distribution from the CMD, run it as administrator and type: 
     ```
     wsl --install --distribution Debian
     ```
   - If you encounter an error trying to start the distribution, try running (this command will update the WSL):
     ```
     wsl --update
     ```

3. Reboot your system.

4. Run WSL as administrator.

Note: If you want to run linux as a Virtual Machine you need to enable the virtualization feature in windows (by following steps explained in step number 1 then enabling virtualization feature, also enable Hyper V)

Note: To find available distributions online run:

        ```
        wsl --list online
        ```

5. **Set Up WSL:**
   - Run the WSL, set a username, and a password. If it doesn’t accept the password, try:
     ```
     <the name you chose> --force-badname
     ```
     and give it a password.

6. **Update the Distribution:**
   - Assuming Ubuntu is installed, run:
     ```
     sudo apt update
     sudo apt dist-upgrade -y
     ```

7. **Access WSL Files from Windows:**
   - Type in the address bar: \\wsl$\ and choose where to put the files on the WSL.

8. **Install Apache Server on WSL:**
   - Install Apache2 by running:
     ```
     sudo apt install apache2 -y
     ```
   - Install the wsgi_mod by running:
     ```
     sudo -H apt install libapache2-mod-wsgi-py3
     ```

9. **Create a Directory:**
   - Create a directory in the home directory by running:
     ```
     mkdir flask_app
     ```

10. **Copy Your Flask Application:**
    - Copy your flask application from Windows to the directory 'flask_app' on the WSL.

11. **Create a Configuration File:**
    - Make a file in this working directory with the name app.wsgi that contains the configuration.

    ``` 
        import sys
        sys.path.insert(0, '/var/www/html/flask_app')
        from app import app as application 
    ```

12. **Install Required Python Packages:**
    - Install the required python packages to run for our first application (Project1) by running:
      ```
      sudo -H pip3 install flask
      sudo -H pip3 install flask_migrate
      sudo -H pip3 install flask_sqlalchemy
      sudo -H pip3 install pymssql
      ```

13. **Copy the Flask Application:**
    - Copy the flask application to the /var/www/html by running:
      ```
      sudo cp -R flask_app/ /var/www/html/
      ```
    - (assuming that the current working directory is the home directory)

14. **Modify the Apache Service Configuration:**
    - Modify the apache service configuration by running:
      ```
      sudo nano /etc/apache2/sites-available/000-default.conf
      ```
      ```
       #DocumentRoot /var/www/html

        WSGIScriptAlias / /var/www/html/flask_app/app.wsgi
        <Directory /var/www/html/flask_app>
        Order allow,deny
        Allow from all
        </Directory>
      ```
15. **Navigate to the Web Flask Application Directory:**
    - Navigate to the web flask application directory by running:
      ```
      sudo cd /var/www/html/flask_app
      ```

16. **Switch to Root User:**
    - Optionally, you can switch to the root user or run the commands with sudo, to switch to root:
      ```
      sudo su - root
      ```

17. **Migrate the Database:**
    - Migrate the database by running:
      ```
      export FLASK_APP=app.py
      flask db init
      flask db migrate -m 'migration'
      flask db upgrade
      ```

18. **Restart the Apache Service:**
    - Restart the apache service by running:
      ```
      sudo service apache2 restart
      ```

Note: If you encounter an installation issue with pymssql on Windows, try to remove it with #pip remove pymssql and then install C++ Build Tools for Windows, or downgrade your pymssql to version 2.2.7 by running: # pip install pymssql==2.2.7

Wishing you all the very best,

Nameer