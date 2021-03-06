# Deploy

* Caution

This turorial is based on [Digital Ocean, Django deploy tutorial, Ubuntu 16.04](https://www.digitalocean.com/community/tutorials/how-to-set-up-django-with-postgres-nginx-and-gunicorn-on-ubuntu-16-04), but not covering full content.
ex, following this instruction, you can't run guinicorn by
```
$ sudo service gunicorn start
```
and you may can't see log file in `/var/log/upstart/gunicorn.log`

1. static file

At first, let me show basic static directory setting in `settings.py`
```
STATIC_URL = '/static/'
STATIC_ROOT = os.path.join(BASE_DIR, 'static')
```
```
$ python manage.py collectstatic
```
about more details, please look [stack overflow](http://stackoverflow.com/questions/8687927/django-static-static-url-static-root)

2. gunicorn
* install
```
$ pip install gunicorn
```
* run
```
$ gunicorn --bind 0.0.0.0:8000 myproject.wsgi:application
```

3. nginx
```
server {
    listen 80;
    server_name my_domain;

    location = /favicon.ico { access_log off; log_not_found off; }
    location /static/ {
        root /home/user/myproject;
    }

    location / {
        include proxy_params;
        proxy_pass http://127.0.0.1:8000;
    }
}
```
4. create gunicorn service

```
$ sudo nano /etc/systemd/system/gunicorn.service
```
```
[Unit]
Description=gunicorn daemon
After=network.target

[Service]
User=jtlim
Group=www-data
WorkingDirectory=/home/jtlim/proj/proj-back
Environment="DJANGO_SETTINGS_MODULE=proj.production_settings"
ExecStart=/home/jtlim/proj/venv/bin/gunicorn --access-logfile - --workers 3 --bind unix:/home/myusername/proj/proj.sock proj.wsgi:application

[Install]
WantedBy=multi-user.target
```

That's all!

This guide has to be updated in future with some content about
```
$ sudo service gunicorn start
```
and should be tested with static files