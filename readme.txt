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

# @147 sshkeygen
choco install openssh

(base) PS C:\Users\aparahnevi> ssh-keygen -t rsa -b 4096
Generating public/private rsa key pair.
Enter file in which to save the key (C:\Users\aparahnevi/.ssh/id_rsa): C:\Users\aparahnevi\.ssh\aws_id_rsa
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in C:\Users\aparahnevi\.ssh\aws_id_rsa.
Your public key has been saved in C:\Users\aparahnevi\.ssh\aws_id_rsa.pub.

go to aws account find EC2 --> Key Pairs --> Create key pair -->
copy/paste content from aws_id_rsa.pub to EC2 key pair

# @148 ssh-add
(base) PS C:\Program Files\OpenSSH-Win64> Start-Service -Name ssh-agent
(base) PS C:\Program Files\OpenSSH-Win64> ssh-add C:\Users\aparahnevi\.ssh\aws_id_rsa
Enter passphrase for C:\Users\aparahnevi\.ssh\aws_id_rsa:
Identity added: C:\Users\aparahnevi\.ssh\aws_id_rsa (venturafoods\aparahnevi@VF109799)
note: 54.176.62.77 copies from aws console of newly created EC2 instance (VPS)
(base) PS C:\Program Files\OpenSSH-Win64> ssh ec2-user@54.176.62.77
The authenticity of host '54.176.62.77 (54.176.62.77)' can't be established.
ECDSA key fingerprint is SHA256:MbhZItufta.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '54.176.62.77' (ECDSA) to the list of known hosts.
   ,     #_
   ~\_  ####_        Amazon Linux 2
  ~~  \_#####\
  ~~     \###|       AL2 End of Life is 2025-06-30.
  ~~       \#/ ___
   ~~       V~' '->
    ~~~         /    A newer version of Amazon Linux is available!
      ~~._.   _/
         _/ _/       Amazon Linux 2023, GA and supported until 2028-03-15.
       _/m/'           https://aws.amazon.com/linux/amazon-linux-2023/

[ec2-user@ip-172-31-10-97 ~]$

# being logged into aws VPS needto getnerate new SSH key for github
[ec2-user@ip-172-31-10-97 ~]$ ssh-keygen -t ed25519 -b 4096
Generating public/private ed25519 key pair.
Enter file in which to save the key (/home/ec2-user/.ssh/id_ed25519):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/ec2-user/.ssh/id_ed25519.
Your public key has been saved in /home/ec2-user/.ssh/id_ed25519.pub.
The key fingerprint is:
key goes here
The key's randomart image is:
+--[ED25519 256]--+
|                 |
|                 |
|                 |
|       .         |
|    o o.S o   .  |
|   = = *+. + . . |
|    B Oo*.o.o   .|
|   . = *.=+o*... |
|    .   EB=BBX=. |
+----[SHA256]-----+
[ec2-user@ip-172-31-10-97 ~]$ cat ~/.ssh/id_ed25519.pub
same key goes here --> copy it over to Deploy Keys in your github for that project!

# @151 copy project over from github repo into aws EC VPS server : using ssh clone
[ec2-user@ip-172-31-10-97 ~]$ git clone git@github.com:lead1974/drf-recipe-app-api.git
Cloning into 'drf-recipe-app-api'...
Warning: Permanently added the ECDSA host key for IP address '140.82.116.4' to the list of known hosts.
Enter passphrase for key '/home/ec2-user/.ssh/id_ed25519':
remote: Enumerating objects: 243, done.
remote: Counting objects: 100% (243/243), done.
remote: Compressing objects: 100% (159/159), done.
remote: Total 243 (delta 120), reused 186 (delta 67), pack-reused 0 (from 0)
Receiving objects: 100% (243/243), 44.64 KiB | 774.00 KiB/s, done.
Resolving deltas: 100% (120/120), done.
