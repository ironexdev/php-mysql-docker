# PHP MySQL Docker
- Rootless containers for your app

## Requirements

- MacOS or Linux, not tested on Windows
- Docker engine 19.03.0+
- docker-compose

## What's in it

- Adminer
- Nginx application server
- Nginx http proxy
- MySQL
- PHP-FPM
- Redis
- Redis Admin

## How To Use

This repository should be used as a submodule.

1) Fork this project to your Git and include it in your main project as submodule

2) Create symlink from main repo to bin of this project m`ain-project/bin/docker` -> `php-mysql-docker/bin`
   - You should now be able to call every from this repo from the root of your project, for example: `bin/docker/compose/up`, `bin/docker/php/fpm pwd`, ...

3) Copy and paste `bin/variables.example.txt` to your main project as `bin/variables` with +x permission

4) Add urls from `images/http-proxy/default.conf` to `etc/hosts` on your machine
   - adminer.pmd.local
   - api.pmd.local
   - pmd.local
   - redis.pmd.local

5) Run bin/compose/secrets to generate mysql credentials used by the app
   - Your can access them from php by reading files
     - `file_get_contents($_ENV["MYSQL_PASSWORD_FILE"])`

6) Run `bin/docker/compose/up` from your main project

7) Folders backend and frontend should now be created in your main project.
   - Add public/index.php to backend folder
   - Add dist/index.html to frontend folder

## Command Reference

- `bin/docker/compose/<command>`
  - Wrappers around docker-compose functions + `bin/docker/compose/secrets`, which generates files containing secrets used by Docker

- `bin/app-server`
  - Run command in app-server container

- `bin/clear-logs`
  - Clear logs folder

- `bin/composer`
  - Run composer command in php-fpm container

- `bin/mysql`
  - Run command in mysql container

- `bin/php-fpm`
  - Run command in php-fpm container

- `bin/xdebug on|off|develop|coverage|debug|gcstats|profile|trace`
  - More info on https://xdebug.org/docs/all_settings#mode