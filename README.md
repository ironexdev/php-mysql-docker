# PHP MySQL Docker
- Rootless containers for your app

## Requirements

- MacOS or Linux, not tested on Windows
- Docker engine 19.03.0+
- docker-compose

## What's in it

- Adminer
- Nginx application server
  - Node
  - Yarn
  - Bind-mounted frontend folder to your host
- Nginx http proxy
- MySQL
- PHP-FPM
  - Composer
  - Bind-mounted backend folder to your host
- Redis
- Redis Admin

## Setup Part 1

This repository should be used as a submodule.

**A) Use it in your own project**

1) Fork this project to your Git and include it in your main project as submodule

2) Create symlink from main repo to bin of this project `main-project/bin/docker` -> `php-mysql-docker/bin`
        - You should now be able to call every from this repo from the root of your project, for example: `bin/docker/compose/up`, `bin/docker/php/fpm pwd`, ...

**B) Use it in example wrapper project**

1) Clone https://github.com/ironexdev/php-mysql-docker-wrapper

2) Run `bin/init` from the wrapper project

## Setup Part 2

1) Copy and paste `bin/variables.example.txt` to your main project as `bin/variables` with +x permission

2) Add urls from `images/http-proxy/default.conf` to `etc/hosts` on your machine
   - `adminer.pmd.local`
   - `api.pmd.local`
   - `pmd.local`
   - `redis.pmd.local`

3) Run bin/compose/secrets to generate mysql credentials used by the app
   - You will probably get some "No such file or directory" errors, but it's ok - check if files were created in secrets folder
   - Your can access them from php by reading files
     - `file_get_contents($_ENV["MYSQL_PASSWORD_FILE"])`

4) Run `bin/docker/compose/up` from your main project

5) Folders backend and frontend should now be created in your main project.
   - Add public/index.php to backend folder
   - Add dist/index.html to frontend folder

## Command Reference

- `bin/docker/compose/<command>`
  - Wrappers around docker-compose functions + `bin/docker/compose/secrets`, which generates files containing secrets used by Docker

- `bin/docker/app-server`
  - Run command in app-server container

- `bin/docker/clear-logs`
  - Clear logs folder

- `bin/docker/composer`
  - Run composer command in php-fpm container

- `bin/docker/mysql`
  - Run command in mysql container

- `bin/docker/php-fpm`
  - Run command in php-fpm container

- `bin/docker/xdebug on|off|develop|coverage|debug|gcstats|profile|trace`
  - More info on https://xdebug.org/docs/all_settings#mode
