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
