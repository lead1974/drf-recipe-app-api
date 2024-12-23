# git to run actions on github
git add .
git commit -am "commit message"
git push origin

docker login -u baldaivanovna
docker build .
# create django project via docker
docker-compose run --rm app sh -c "django-admin startproject app ."
docker-compose run --rm app sh -c "flake8"
docker-compose up

