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
docker-compose run --rm app sh -c "python manage.py test"

# run command
docker-compose run --rm app sh -c "python manage.py wait_for_db"
