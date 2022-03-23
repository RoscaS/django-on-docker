# Base dockerized django project
* [docker-compose good practices](https://mherman.org/presentations/dockercon-2018)
* [Details](https://testdriven.io/dockerizing-django-with-postgres-gunicorn-and-nginx)

## Development

Uses the default Django development server.

1. Rename *.env.dev-sample* to *.env.dev*.
2. Update the environment variables in the *docker-compose.yml* and *.env.dev* files.
3. Build the images and run the containers:

    ```sh
    $ docker-compose up -d --build
    ```

    Test it out at [http://localhost:8000](http://localhost:8000). The "app" folder is mounted into the container and your code changes apply automatically.

## Production (gunicorn + nginx)

1. Rename *.env.prod-sample* to *.env.prod* and *.env.prod.db-sample* to *.env.prod.db*. Update the environment variables.
2. Build the images and run the containers:

    ```sh
    $ docker-compose -f docker-compose.prod.yml up -d --build
    ```

    Test it out at [http://localhost:1337](http://localhost:1337). No mounted folders. To apply changes, the image must be re-built.
    Notice that [http://localhost:1337/admin](http://localhost:1337/admin) style is working 

### Migrations, Static & Media

   ```sh
   $ docker-compose -f docker-compose.prod.yml exec web python manage.py migrate --noinput
   $ docker-compose -f docker-compose.prod.yml exec web python manage.py collectstatic --no-input --clear
   ```

## Misc
* spin down: `docker-compose down -v`
* 'active endpoints error': `docker-compose down -v --remove-orphans`
