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
2. Run composer inside nginx with your_project_name
```bash
docker exec nginx composer create-project --prefer-dist laravel/laravel /var/www/html/your_project_name
```
**Step 1 and 2 must have the same name**

# Permission issues
Often when we share code on a volume from our machine to the docker, the container changes the unix user who owns the files and folders. This makes it difficult for us to edit the code and continue with the development. To patch this we can do 2 things:

* Open code with sudo.
* Change the unix user with 'chown -r user:user code/'.

This is good. But repeating it every time we deploy the project is somewhat boring.
To fix this, in our nginx we can include an environment variable in the **docker-compose.yml**, which when starting the container sets the local UID of our user to the UID of the service user. In this way, when our Unix system verifies the owner of the files, it identifies the same id and allows us to edit without problems.
```bash
environment:
  - PUID=user_unix_id
```
To know the UID of your unix user type '**id -u**' or type '**echo $UID**' or **read in /etc/passwd** in your terminal.
