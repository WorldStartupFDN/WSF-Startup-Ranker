### WSF Startup Ranker

## Deployment

[![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy?template=https://github.com/WorldStartupFDN/WSF-Startup-Ranker/tree/master)

The latest stable version is the `master` branch (and it's signed and tagged).
Development happens in the `develop` branch.

The web application is written in **Python 3** using Flask. It also uses NumPy
and SciPy for math stuff. Doing a `pip install -r requirements.txt` should
install all the dependencies.

The application uses Postgres for the database, so you need to have that on
your server. You need to create a database, which you can do with `createdb
gavel` (unless you're using a different database name). Before you use the app,
you need to initialize the database by running `python initialize.py`. **Note
that Gavel does not preserve database schema compatibility between versions.**

In order to send emails, you'll need to install Redis.

When testing, you can run the app with `python runserver.py`. In production,
you should use something like [Gunicorn][gunicorn] to serve this. You can run
the app with `gunicorn -b :<PORT> -w <number of workers> gavel:app`.

For sending emails, you'll also need to start a celery worker with `celery -A
gavel:celery worker`.

## Configuration

Before starting the app, copy `config.template.yaml` to `config.yaml` and set
all the required settings (the ones that don't have default values).

Most settings can either be set in `config.yaml` or set as environment
variables. There's more detailed documentation in `config.template.yaml`.

If you don't want to use the config file and use only environment variables,
set the environment variable `IGNORE_CONFIG_FILE=true`.

## Use

To set up the system, use the admin interface on `/admin`. Log in with the
username `admin` and the password you set. Once you're logged in, you can input
information for all the projects and judges.

As you add judges, they'll automatically get emails with invitation links.
After that, the judging and ranking process is fully automated - the judge will
be able to read the welcome text, and then they'll be able to start judging.

The admin panel will rank projects in real time, ordered by their inferred
quality (Mu).

### Admin Panel Features

* If you want to (temporarily) close the judging system, click the "Close"
  button under "Global Settings"
* If you need to force re-send the invite email, use the "Email" button for the
  judge in the admin panel
* If you need to manually give a judge a login link, direct them to
  `/login/<secret>`
* If you want to send the next available judge to a certain project, use the
  "Prioritize" button
* If you need to deactivate projects or judges at any point, use the "Disable"
  button
* If a project hasn't been judged yet, you can delete it using the "Delete"
  button
* If a judge hasn't started yet, you can delete them using the "Delete" button
* If you need to see details for a project or judge, click on the item ID in
  the admin panel
    * If you need to edit a project (name, location, or description), you can
      do so on the item detail page
* If you want to sort the items in the admin panel, click on the table headers
