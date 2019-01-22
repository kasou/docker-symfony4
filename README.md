# docker-symfony4
Docker stack for the symfony latest version

## script bash for create project
```bash
wget https://github.com/jacquesndl/docker-symfony4/archive/master.zip
unzip master.zip && rm master.zip
mv docker-symfony4-master/* . && rm -R docker-symfony4-master
cp docker/db/.env.dist docker/db/.env && cp docker/php/.env.dist docker/php/.env
docker-compose up -d --build && docker-compose exec php composer create-project symfony/website-skelet$
sudo chown 1000:1000 -R .
sudo chmod 777 -R .
rm -R var
mv app/* app/.[^.]* . && rm -R app
echo MAILER_URL=smtp://mailcatcher:1025 >> .env.local
echo DATABASE_URL=mysql://user:password@db:3306/database >> .env.local
```

## script bash for install project
```bash
cp docker/db/.env.dist docker/db/.env && cp docker/php/.env.dist docker/php/.env
docker-compose up -d --build
docker-compose exec php composer install
sudo chown 1000:1000 -R .
sudo chmod 777 -R .
echo MAILER_URL=smtp://mailcatcher:1025 >> .env.local
echo DATABASE_URL=mysql://user:password@db:3306/database >> .env.local
```

### dev
http://localhost/

### mailcatcher
http://localhost:1080/

### adminer
http://localhost:8080/

### composer
```
docker-compose exec php composer [...]
docker-compose exec php composer install
```

### tests
```
docker-compose exec php bin/phpunit [...]
docker-compose exec php bin/phpunit
```

### php-cs-fixer
```
docker run --rm --tty --volume $PWD:/app jacquesndl/php-cs-fixer [...]
docker run --rm --tty --volume $PWD:/app jacquesndl/php-cs-fixer fix src/ --rules=@PSR1,@PSR2,@Symfony
```

### phpstan
```
docker run --rm --tty --volume $PWD:/app jacquesndl/phpstan [...]
docker run --rm --tty --volume $PWD:/app jacquesndl/phpstan analyse src/ --level=7
```
