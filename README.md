python runtime setup
--
````
pyenv install 3.4.1
pyenv virtualenv 3.4.1 django-sandbox-3.4.1
# pyenv local django-sandbox-3.4.1
pip install django
pip install --allow-external mysql-connector-python mysql-connector-python
# pip freeze > requirements.txt
pip install --allow-external mysql-connector-python -r requirements.txt
````
`mysql-python`(MySQLdb) can not be used with python 3, so use `mysql-connector-python`.

package setup
--
````
brew install mysql
````

check installation
--
````
python -c "import django; print(django.get_version())"
````

create django project
--
````
pyenv rehash
django-admin.py startproject hellosite
````

create database
--
````
mysql.server start
mysql -uroot
CREATE DATABASE hellosite;
GRANT ALL ON hellosite.* TO hello@localhost IDENTIFIED BY 'hello';
FLUSH PRIVILEGES;
````

````
mysql -uhello -p
show databases;
use hellosite;
````

configure django project database
--
````
cd hellosite/hellosite/
vi settings.py
````
````
# Database
# https://docs.djangoproject.com/en/1.7/ref/settings/#databases

DATABASES = {
    'default': {
        # 'ENGINE': 'django.db.backends.mysql',
        # use MySQL Connector/Python, instead of MySQLdb
        'ENGINE': 'mysql.connector.django',
        'NAME': 'hellosite',
        'USER': 'hello',
        'PASSWORD': 'hello',
        'HOST': '127.0.0.1',
        'PORT': '3306',
    }
}
````
````
python manage.py migrate
````

run test server
--
````
python manage.py runserver
open 'http://localhost:8000/'
````

use admin site
--
````
python manage.py createsuperuser
open 'http://localhost:8000/admin'
````

note
==
brew mysql installation note
--
````
A "/etc/my.cnf" from another install may interfere with a Homebrew-built
server starting up correctly.

To connect:
    mysql -uroot

To have launchd start mysql at login:
    ln -sfv /usr/local/opt/mysql/*.plist ~/Library/LaunchAgents
Then to load mysql now:
    launchctl load ~/Library/LaunchAgents/homebrew.mxcl.mysql.plist
Or, if you don't want/need launchctl, you can just run:
    mysql.server start
````

open project w/ intellij
--
* `open project`
* `project structure` in `files`
    * in `platform settings`
        * set virtualenv environment to `sdks` 
    * in `modules` at `project settings`
        * add django module like `hellosite/hellosite` as `Django`'s one
            * project name; `hellosite`
            * application name; `hellosite`
            * templates folder; `/path/to/hellosite`
            * module name; `hellosite`
            * content root; `/path/to/hellosite`
            * module file location; `/path/to/hellosite/hellosite`
        * configure the module like below
            * django project root; `/path/to/hellosite/`
            * settings; `hellosite/settings.py`
            * manage script; `manage.py`
        * set module located top of list like `django-sandbox`'s dependency to project virtual env
* `edit configurations` in `run`
    * remove `make` command in `before launch`
* check commands in `tools`
    * `run django console`
    * `run manage tasks`

