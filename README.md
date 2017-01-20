# LinuxServerConfiguration
This is the seventh project for the "Full Stack Web Developer".

Here a Linux Virtual Machine was configured to support the Item Catalog Project.

Please visit http://52.25.0.212/ for the website deployed.

## Configuring and Lauching the application

#### Accessing the linux machine
1. Create a remote development environment.
2. Download your private key.
3. Move your key(udacity_key.rsa) to `~/.ssh`, where ~ is your home's directory, then type `cd ~/.ssh` .
4. Change file's permission by typing `chmod 600 udacity_key.rsa` .
5. Finally to access the remote linux type `ssh -i udacity_key.rsa root@52.25.0.212`

#### Creating a new user.
1. Type `sudo adduser grader` then configure his/her password, name, location, etc.

#### Changing user permission.
1. Create a file inside sudoers.d by typing `touch /var/sudoers.d/grader`.
2. `sudo nano /var/sudoers.d/grader`.
3. Inside this file type `grader ALL=(ALL) NOPASSWD:ALL`, then save and quit.

#### Creating a new private/public pair.
1. `su - grader`.
2. `mkdir ~/.ssh`.
3. `touch ~/.ssh/authorized_keys`
4. `ssh-keygen -t rsa` you can just press enter for each information, leaving them empty.
5. Two file will be created (inside ~/.ssh) `id_rsa` and `id_rsa.pub` if nothing was changed.
6. `cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys`.
7. `id_rsa` will be your private key to access the VM using `grader`
8. After copying `id_rsa` to you local machine `chmod 600 id_rsa`.
9. `ssh -i id_rsa grader@52.25.0.212`.

#### Updating/Upgrading the VM.
1. `sudo apt-get update`.
2. `sudo apt-get upgrade`.

#### Changing ssh port to 2200.
1. `sudo nano /etc/ssh/sshd_config`
2. Locate the Port number and change it to 2200.
3. `sudo service ssh restart`.

#### Configuring UFW.
1. `sudo ufw allow www/tcp`.
2. `sudo ufw allow 2200/tcp`.
3. `sudo ufw allow ntp/udp`.
4. `sudo ufw enable`.

#### Configuring local timezone.
1. `sudo dpkg-reconfigure tzdata`.
2. Change to UTC.

#### Installing Apache and WSGI.
1. `sudo apt-get install apache2`.
2. `sudo apt-get install libapache2-mod-wsgi python-setuptools`.

#### Installing PostgreSQL.
1. `sudo apt-get install postgresql`.

#### Configuring PostgreSQL.
1. Check if remote connection are disabled, type `sudo nano /etc/postgresql/9.3/main/pg_hba.conf`.
2. `sudo su - postgres`.
3. `psql`.
4. Create a new database and configure new user.

```
CREATE DATABASE catalog;
CREATE USER catalog;
ALTER ROLE catalog WITH PASSWORD '123asd'
GRANT ALL PRIVILEGES ON DATABASE catalog TO catalog
exit
```

#### Installing GIT and and setting up Item Catalog Project
1. `sudo apt-get install git`
2. `sudo mkdir /var/www/FlaskApp`
3. `cd /var/www/FlaskApp`
4. `sudo git clone https://github.com/shuminyang/ItemCatalogProject`
5. Change ItemCatalogProject to FlaskApp by typing `sudo mv ItemCatalogProject/ FlaskApp/`
6. `cd FlaskApp\`
7. Rename item_catalog_project.py to __init__.py `sudo mv item_catalog_project.py __init__.py`
8. Edit __init__.py, database_setup.py and database_init.py and change
`engine = create_engine('sqlite:///itemcatalogwithcategory.db')` 
to
`engine = create_engine('postgresql://catalog:123asd@localhost/catalog'`
9. Install pip `sudo apt-get install python-pip`
10. Create a file dependencies `touch dependencies`
11. `sudo nano dependencies` and paste 
```

```
