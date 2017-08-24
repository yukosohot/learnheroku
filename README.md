# learnheroku
use cloud9 (ide.c9.io) as learning environment
## set up
go to cloud9 console
* update heroku-cli by using
```
wget -qO- https://cli-assets.heroku.com/install-ubuntu.sh | sh
```
> ref (https://devcenter.heroku.com/articles/heroku-cli#debian-ubuntu)
* run following commands
```
sudo pip install virtualenv
virtualenv venv
source ./venv/bin/activate
pip install Django
pip install dj-database-url
pip install gunicorn
pip install psycopg2
pip install whitenoise
django-admin startproject learnheroku
cd learnheroku
pip freeze > requirements.txt
```
## heroku files
* create Procfile with this inside
```
web: gunicorn learnheroku.wsgi --log-file -
```
* create app.json with this inside
```javascript
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
```
* prepare static folder for post-run as define with scripts in app.json when deploy on heroku
```
cd learnheroku
mkdir static
cd static
echo ' ' > humans.txt
```
> static folder must be beside settings.py
> humans.txt is just for make folder not empty then git will take it
## edit django settings.py
* import dj_database_url
* update BASE_DIR
```python
BASE_DIR = os.path.dirname(os.path.dirname(__file__))
PROJECT_ROOT = os.path.dirname(os.path.abspath(__file__))
```
* update allow host
```python
ALLOWED_HOSTS = ['*']
```
* update middleware
```python
MIDDLEWARE = [
    'whitenoise.middleware.WhiteNoiseMiddleware',
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.auth.middleware.SessionAuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]
```
* update static folder
```python
STATIC_ROOT = os.path.join(PROJECT_ROOT, 'staticfiles')
STATIC_URL = '/static/'

# Extra places for collectstatic to find static files.
STATICFILES_DIRS = (
    os.path.join(PROJECT_ROOT, 'static'),
)

STATICFILES_STORAGE = 'whitenoise.storage.CompressedManifestStaticFilesStorage'
```
