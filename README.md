### LAMP-role-ansible
#### *configuring LAMP stack using roles in ansible*

##### Change working directory to /etc/ansible/roles and create a Role directory
`# ansible-galaxy init lamp`

##### Define the variables, templates, files, tasks in their respective places. Needs to create roles as the root user
```
# tree lamp
lamp
├── defaults
│   └── main.yml
├── files
│   ├── info.html
│   └── info.php
├── handlers
│   └── main.yml
├── meta
│   └── main.yml
├── tasks
│   └── main.yml
├── templates
│   └── virtualhost.tmpl
├── tests
│   ├── inventory
│   └── test.yml
└── vars
    └── main.yml
```
> __*Defining variables*__
```
$ cat lamp/vars/main.yml 

---
domain: www.devopsarju.com

mysql_root: mysqlroot123
mysql_database: wordpress
mysql_user: wpuser
mysql_password: wpuser123
```
> __*Creating template file*__
```
$ cat lamp/templates/virtualhost.tmpl

<virtualhost *:80>
  servername {{ domain }}
  documentroot /var/www/html/{{ domain }}
  directoryindex index.php index.html info.html info.php
</virtualhost>
```
> __*Creating info.html and info.php*__
```
$ cat lamp/files

echo "<h1>It works </h1>"  > lamp/files/info.html
echo "<?php phpinfo(); ?>" > lamp/files/info.php
```

##### Add all the tasks to lamp/tasks/main.yml and exit from the root

> __*Create a wp-config.php.tmpl of the wp conf file and edit db_name,db_user,db_password and db_hostname*__
```
/** The name of the database for WordPress */
define( 'DB_NAME', '{{ mysql_database }}' );
/** MySQL database username */
define( 'DB_USER', '{{ mysql_user }}' );
/** MySQL database password */
define( 'DB_PASSWORD', '{{ mysql_password}}' );
/** MySQL hostname */
define( 'DB_HOST', 'localhost' );
```
##### Now create playbook to install wordpress and define roles
