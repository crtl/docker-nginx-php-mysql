# NGINX / PHP / Docker Configuration

NGINX, PHP, MariaDB stack with docker compose for development

---

1. [Prerequisites](#prerequisites)
2. [Clone](#clone)
3. [Configuration](#configuration)
4. [Additional PHP Extensions](#additional-php-extensions)
5. [Start](#start)
6. [Servers](#servers)
8. [Useful Commands](#useful-commands)
9. [Contributing](#contributing)

---

## Prerequisites

1. [Docker / Docker Desktop](https://www.docker.com/products/docker-desktop/)
2. [Git](https://git-scm.com/)
4. [WSL2 on Windows](https://learn.microsoft.com/de-de/windows/wsl/install)

---

## Clone

1. Clone using `git clone git@github.com:crtl/docker-nginx-php-mysql.git`
2. Delete .git folder `rm -rf .git`
3. Run `git init`
6. Start dupluping

---

## Configuration

To configure docker services edit environment variables inside `.docker/.env` file.<br/> 
For more information check [Docker Documentation - Use an environment file in Docker Compose](https://docs.docker.com/compose/environment-variables/env-file/)

| ENV_NAME        | Default       | Description                 |
|-----------------|---------------|-----------------------------|
| MARIADB_VERSION | 10            | MariaDB Version             |
| PHP_VERSION     | 8.1           | PHP Version                 |
| PHP_TIMEZONE    | Europe/Berlin | Timezone for PHP service    |
| SERVER_PORT     | 80            | Port for http server        |
| PHPMYADMIN_PORT | 8080          | Port for phpmyadmin service |

---

## Additional PHP Extensions

- xdebug
- opcache
- gd
- zip
- pdo_mysql
- intl
- iconv
- bcmath

To install/enable extensions edit [`.docker/php/Dockerfile#L33`](.docker/php/Dockerfile#L33)<br/>
For information on how to install more extensions check [How to install more PHP extensions](https://hub.docker.com/_/php#:~:text=How%20to%20install%20more%20PHP%20extensions)

--- 

## Start

1. Run `docker compose --env-file .docker/.env up -d`
2. Go to http://localhost

### MySQL Database

To automatically import database dumps on service creation place `*.sql` and/or `*.sql.gz` files into `./docker/mysql/initdb.d` directory.
More information can be found in [image documentation for the mysql image - Initializing a fresh instance](https://hub.docker.com/_/mysql#:~:text=Initializing%20a%20fresh%20instance)


---

## Servers

| Server | Description       |
| ------ |-------------------|
| [localhost](http://localhost) | NGINX server      |
| [PHPMyAdmin](http://localhost:8080) | PHPMyAdmin server |

---


## Useful Commands

| Command                            | Description               |
|------------------------------------|---------------------------|
| `docker compose exec -it db bash`  | Access MariaDB shell      |
| `docker compose exec -it app bash` | Access app instance shell |

---

## Contributing

To contribute:

1. Fork this repository
2. Create a feature branch `git checkout -b feature/my-new-feature`
3. Commit changes
4. Send pull request to `master`
