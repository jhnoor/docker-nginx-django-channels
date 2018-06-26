# docker-nginx-django-channels
Simple example showing how django channels can work behind nginx

# Getting started

Run `docker-compose up --build -d`, omitting -d if you want to be fed the logs immediately (remember ctrl + z will exit out of logs without killing the containers).

Then run `docker-compose run django_asgi python manage.py migrate` for the initial migration. This can also be achieved by entering the container via `docker exec -it django_asgi bash` and then running the commands.

and finally run `docker-compose run django_asgi python manage.py createsuperuser`

# How it works

The `default.conf` nginx configuration shows how certain paths are mapped to either the wsgi worker or the asgi worker:
    
- `/` maps to uwsgi. Meaning it passes on the request via `uwsgi_pass` to `django_wsgi`, which internal DNS lookup points to the django_wsgi container
- `/chat/stream` maps to asgi. This uses proxy_pass to conserve the websocket and passes to `http://django_asgi`, which maps to the django_asgi container. 

# Credits

This repo is based on [Andrew Godwin's channels examples](https://github.com/andrewgodwin/channels-examples).