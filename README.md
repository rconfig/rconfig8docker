<!-- References:
https://www.twilio.com/blog/get-started-docker-laravel
https://laravel-for-newbie.kejyun.com/en/advanced/scheduling/docker/
https://github.com/mohammadain/laravel-docker-cron/blob/master/Dockerfile
-->

<a name="readme-top"></a>

<!-- PROJECT LOGO -->
<br />
<div align="center">
  <a href="https://github.com/rconfig/rconfig">
    <img src="https://portal.rconfig.com/images/rconfig-logos/rConfig-logo-and-icon/rconfig_logo_and_icon_with_strapline_gradient.png" alt="rConfig Logo" width="500"/>
  </a>

  <h3 align="center">
    rConfig V8 Professional Docker Compose Setup
    <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/docker/docker-original.svg" width="35" height="35" />
  </h3>

  <p align="center">
    rConfig V8 Professional is an enterprise-grade Network Configuration Management (NCM) software package. It simplifies the management of configurations across large and small heterogeneous networks. This repository contains the rConfig V8 Professional codebase and scripts to deploy it with Docker Compose.
    <br /><br />
    <a href="https://docs.rconfig.com"><strong>Explore the docs »</strong></a>
    <br /><br />
    <a href="#intro">Intro</a> ·
    <a href="#setup">Installation</a> ·
    <a href="#update">Updating</a> ·
    <a href="#license">License</a> ·
    <a href="#support">Support</a>
  </p>

  <!-- Badges -->
  [![Tests](https://github.com/eliashaeussler/typo3-badges/actions/workflows/tests.yaml/badge.svg)](https://github.com/eliashaeussler/typo3-badges/actions/workflows/tests.yaml)
  <!-- Technologies -->
  <p>
    <!-- Backend & Docker -->
    <img src="https://img.shields.io/badge/-Docker-2496ED?logo=docker&logoColor=white&style=flat" />
    <img src="https://img.shields.io/badge/-PHP-777BB4?logo=php&logoColor=white&style=flat" />
    <img src="https://img.shields.io/badge/-Laravel-FF2D20?logo=laravel&logoColor=white&style=flat" />
    <img src="https://img.shields.io/badge/-Composer-2F4F4F?logo=composer&logoColor=white&style=flat" />
    <img src="https://img.shields.io/badge/-MySQL-4479A1?logo=mysql&logoColor=white&style=flat" />
    <img src="https://img.shields.io/badge/-Redis-DC382D?logo=redis&logoColor=white&style=flat" />
  </p>

  <p>
    <!-- Frontend -->
    <img src="https://img.shields.io/badge/-Vue3-4FC08D?logo=vue.js&logoColor=white&style=flat" />
    <img src="https://img.shields.io/badge/-ViteJs-6e37a0?logo=vite&logoColor=white&style=flat" />
    <img src="https://img.shields.io/badge/-PatternFly-004285?logo=patternfly&logoColor=white&style=flat" />
    <img src="https://img.shields.io/badge/-Node.js-339933?logo=node.js&logoColor=white&style=flat" />
    <img src="https://img.shields.io/badge/-npm-CB3837?logo=npm&logoColor=white&style=flat" />
    <img src="https://img.shields.io/badge/-Yarn-2C8EBB?logo=yarn&logoColor=white&style=flat" />
  </p>
</div>

<a name="intro"></a>


## Intro

rConfig V8 Docker Compose Setup is a set of scripts to help you get started with rConfig V8 Professional on Docker. rConfig V8 Professional is an enterprise grade Network Configuration Management (NCM) software package with superior NCM features and capabilities to help you easily manage configurations on large and small heterogenous networks. rConfig V8 Professional runs natively on many variants of Linux.

If you are looking for rConfig V6 Core, please visit `https://github.com/rconfig/`.

## rConfig Editions

| Feature              | rConfig Core        | rConfig Professional |
|----------------------|---------------------|----------------------|
| Configuration Backup | :white_check_mark:  | :white_check_mark:   |
| Multi-Vendor Support | :white_check_mark:  | :white_check_mark:   |
| Unlimited Devices | :white_check_mark:  | :white_check_mark:   |
| Enterprise Features  | :x:                 | :white_check_mark:   |
| API Access           | :x:                 | :white_check_mark:   |
| Premium Support      | :x:                 | :white_check_mark:   |

_Check out more on our [features page](https://www.rconfig.com/pricing#full-features)._

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- Installation -->

<a name="setup"></a>

## Installation
 
> [!NOTE]  
> rConfig V8 Professional is a paid product. You can purchase a subscription from [rConfig](https://www.rconfig.com/pricing). It Requires a valid license key to download and 
run the docker image. Please contact rConfig for more information at [info@rconfig.com](mailto:info@rconfig.com).

### Prerequisites

- Docker & Docker Compose installed on your server
- The data directory should be writable by the web server user in the docker container, and the contents of the data directory in this repository should be copied to the data directory on your server. YOu may clone this repository and copy the contents of the data directory to your server if you wish.

> **Note**
> You will need to be logged in as root when running the scripts.

### Steps

1. Clone this repository to your server
2. Login to the rConfig private registry. You will need to have a valid subscription to rConfig Professional to access the registry. Credentials are provided by rConfig.

```sh
    docker login reg.rconfig.com:8443
    Username: robot$rconfig+equans
    Password:  #Enteryourcredentials
```
 
3. Locate the `.env` file in the `data` directory and update the following values:

```sh
    APP_DEBUG=true/false
    APP_URL=http://youFQDNorIP.domain.com
    DB_HOST=db # This is the name of the database container or other server if you are using an external database
    DB_PORT=ENVOY_DB_PORT # The port of the database server
    DB_DATABASE=ENVOY_DB_DATABASE # The name of the database, this can be anything if you are creating a new database
    DB_USERNAME=ENVOY_DB_USERNAME # The username for the database
    DB_PASSWORD=ENVOY_DB_PASSWORD # The password for the database
    RCONFIG_API_TOKEN=ENVOY_API_TOKEN # The API token for the rConfig API
```

4. Run the following command to start the rConfig V7 Professional Docker container:

```sh
    docker-compose up -d
```

5. Verify that the container is running by running the following command:

```sh
    docker ps
```

6. You need to complete the installation by running the following commands inside the container:

```sh
    docker exec -it rconfig-container-name bash
    php artisan migrate
    php artisan key:generate
    php artisan passport:install # Please answer yes to the questions
    php artisan rconfig:sync-tasks
    php artisan rconfig:sync-search 
    php artisan rconfig:clear-all
    php artisan rconfig:update-rbac-data  
    chown -R www-data:www-data /var/www/html/rconfig8/current/storage
    chmod -R 775 /var/www/html/rconfig8/current/storage
```

7. Restart the container:

```sh
    docker compose up -d
```

8. You can now access rConfig V7 Professional by navigating to the URL you specified in the `.env` file.
9. You can now login with the default credentials:

```sh
    Username: admin@domain.com
    Password: admin
```

10. Verify that the installation was successful by navigating to the `Settings` page and checking the version number.
11. Verify Horizon is running by navigating to the `Settings > Queue Manager` page. The status on the Dashboard should be `active`.


<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- LICENSE -->

<a name="license"></a>

## License

This code base for this repository's code is distributed under License from rConfig. See `LICENSE.txt` for more information. rConfig v6 Professional is excluded from this license and repository.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- https://github.com/othneildrew/Best-README-Template/blob/master/README.md -->

<a name="support"></a>

## Support

rConfig V7 Professional is an enterprise grade Network Configuration Management (NCM) software package with superior NCM features and capabilities to help you easily manage configurations on large and small heterogenous networks. Users with subscriptions to rConfig Professional can contact rConfig for support at [info@rconfig.com](mailto:support@rconfig.com).

## Acknowledgments

Inspiration, code snippets, etc.

- [Laravel](https://www.laravel.com)
- [vuejs](https://vuejs.org/)
- [patternfly v5](https://www.patternfly.org/)

See composer.json and package.json for a full list of dependencies, and their licenses.
