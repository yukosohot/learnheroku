# learnheroku
use cloud9 (ide.c9.io) as learning environment

## set up
go to cloud9 console

* update heroku-cli by using

wget -qO- https://cli-assets.heroku.com/install-ubuntu.sh | sh

ref (https://devcenter.heroku.com/articles/heroku-cli#debian-ubuntu)

* sudo pip install virtualenv
* virtualenv venv
* source ./venv/bin/activate
* pip install Django
* pip install dj-database-url
* pip install gunicorn
* pip install psycopg2
* pip install whitenoise
* django-admin startproject learnheroku
* cd learnheroku
* pip freeze > requirements.txt

## heroku files
* create Procfile with this inside

web: gunicorn learnheroku.wsgi --log-file -

* create app.json with this inside

{
  "name": "learnheroku",
  "description": "My heroku learning",
  "image": "heroku/python",
  "repository": "https://github.com/yukosohot/learnheroku.git",
  "keywords": ["python", "django" ],
  "addons": [ "heroku-postgresql" ],
  "environments": {
    "test": {
      "scripts": {
        "test-setup": "python manage.py collectstatic --noinput",
        "test": "python manage.py test"
      }
    }
  }
}

