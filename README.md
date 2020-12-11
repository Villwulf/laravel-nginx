# Overview
Deploying laravel project into nginx using docker-compose.

# Running
To simply run all the containers:
```bash
docker-compose up -d
```
# Make Laravel Project
Run composer inside nginx container to create laravel project:
```bash
docker exec nginx composer create-project --prefer-dist laravel/laravel /var/www/html/laravel
```
# Develop
Just go to code/ and enjoy.

# Another project name
If you want a different name for your project, you only have to make 2 changes:

1. Before start dockers... change one line on conf.d/default.conf:
```conf
root /var/www/html/laravel/public;
```
for this:
```conf
root /var/www/html/your_project_name/public;
```
