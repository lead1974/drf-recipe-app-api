# git to run actions on github.
git add .
git commit -am "commit message"
git push origin

docker login -u baldaivanovna
docker build .
# create django project via docker
docker-compose run --rm app sh -c "django-admin startproject app ."
docker-compose run --rm app sh -c "flake8"
docker-compose up

# new django app 'core'
docker-compose run --rm app sh -c "python manage.py startapp core"

# run test coammand
docker-compose run --rm app sh -c "python manage.py test && flake8"

# run command
docker-compose run --rm app sh -c "python manage.py wait_for_db"

# @ 47 test user model tests/test_models.py
docker-compose run --rm app sh -c "python manage.py test && flake8"

# @ 48 makemigraitons
docker-compose run --rm app sh -c "python manage.py makemigrations"
docker-compose run --rm app sh -c "python manage.py wait_for_db && python manage.py migrate"
docker volume ls
docker volume rm drf-recipe-app-api_dev-db-data


# @ 51 create superuser
docker-compose run --rm app sh -c "python manage.py createsuperuser"

# @66 create user app (API)
docker-compose run --rm app sh -c "python manage.py startapp user"

# @79 creating Recipe model
docker-compose run --rm app sh -c "python manage.py makemigrations && python manage.py migrate"
# @80 creating Recipe app
docker-compose run --rm app sh -c "python manage.py startapp recipe"
 1. delete recipe/admin.py, recipe/models.py, recipe/tests.py
 2. create recipe/tests/__init__.py
 3. app/settings.py add 'recipe' to INSTALLED_APPS

# @91 creating Tags model
docker-compose run --rm app sh -c "python manage.py makemigrations && python manage.py migrate"

# @127 imap upload API
swagger image upload requires in settings.py:  'COMPONENT_SPLIT_REQUEST': True,

# @141 after building proxy/Dockerfile need to run
cd proxy
docker build .

<<<<<<< HEAD
# @144 after update django settings.py we need to test our deployement setup
docker-compose -f docker-compose-deploy.yml down
docker-compose -f docker-compose-deploy.yml up
=======
>>>>>>> 6ffc037508bbae42a5a034ec00a2c681f7dc539f


