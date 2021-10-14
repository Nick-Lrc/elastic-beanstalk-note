# Elastic Beanstalk Note

## How does Elastic Beanstalk serve your application?
- It uses the [Web Server Gateway Interface](http://wsgi.readthedocs.org/en/latest/) (WSGI) server.
- [Gunicorn](https://gunicorn.org/) is the default WSGI server.

## How does [Gunicorn](https://gunicorn.org/) run your application?
- It reads the `WSGI_APP` argument in the terminal.
- The pattern of this argument is `$(MODULE_NAME):$(VARIABLE_NAME)`. e.g. `app:app`.
- The `MODULE_NAME` is the name of the script to run your application. e.g. `app` in `app.py`.
- The `VARIABLE_NAME` is the callable object in this script. e.g. `app` in `app = Flask(__name__)`.
- For Elastic Beanstalk, the argument is `application:application` by default. This setting causes failure of deploying some applications.

## How to overwrite the `WSGI_APP` argument set by Elastic Beanstalk?
- Use a [Procfile](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/python-configuration-procfile.html). It configs the WSGI server for your application.
- In the `Procfile`, modify the web attribute to make gunicorn accept a different `WSGI_APP` argument. e.g. `web: gunicorn --bind :8000 --workers 3 --threads 2 app:app`.

## Why use `zip -r -X <your_archive_name>.zip *` in the base directory?
- So the archive will not wrap application files with an additional folder.
- Otherwise Elastic Beanstalk will not find your application.

## Extra resources
- [Deploying a Flask application to Elastic Beanstalk](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/create-deploy-python-flask.html)
- [Configuring the WSGI server with a Procfile](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/python-configuration-procfile.html)
- [Gunicon Settings](https://docs.gunicorn.org/en/stable/settings.html#config)
- [zip(1) - Linux man page](https://linux.die.net/man/1/zip)
