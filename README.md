<!-- References:
https://www.twilio.com/blog/get-started-docker-laravel
https://laravel-for-newbie.kejyun.com/en/advanced/scheduling/docker/
https://github.com/mohammadain/laravel-docker-cron/blob/master/Dockerfile -->

<!-- Improved compatibility of back to top link: See: https://github.com/othneildrew/Best-README-Template/pull/73 -->

<a name="readme-top"></a>

<!-- PROJECT LOGO -->
<br />
<div align="center">
  <a href="https://github.com/rconfig/rconfig">
            <img src="https://www.rconfig.com/images/rConfig_logos/new/blue/hex_logo_blue_horizontal_96.png" alt="rConfig Logo" />
  </a>

  <h3 align="center">rConfig V7 Professional Docker Compose Setup</h3>

  <p align="center">
    rConfig V7 Professional is an enterprise grade Network Configuration Management (NCM) software package with superior NCM features and capabilities to help you easily manage configurations on large and small heterogenous networks. rConfig V7 Professional runs natively on many variants of Linux. Within this repo, is the code base for rConfig V7 Professional, and a set of scripts to help you get started with rConfig V7 Professional on Docker.
    <br />
    <br />
    <a href="https://docs.rconfig.com"><strong>Explore the docs »</strong></a>
    <br />
    <br />
    <a href="#intro">Intro</a>
    ·
    <a href="#setup">Installation</a>
    ·
    <a href="#update">Updating</a>
    ·
    <a href="#license">License</a>
    ·
    <a href="#support">Support</a>
  </p>

[![Tests](https://github.com/eliashaeussler/typo3-badges/actions/workflows/tests.yaml/badge.svg)](https://github.com/eliashaeussler/typo3-badges/actions/workflows/tests.yaml)
[![License](https://img.shields.io/github/license/eliashaeussler/typo3-badges)](LICENSE) [![Made with Node](https://img.shields.io/badge/dynamic/json?label=node&query=%24.engines%5B%22node%22%5D&url=https%3A%2F%2Fraw.githubusercontent.com%2FMichaelCurrin%2Fbadge-generator%2Fmaster%2Fpackage.json)](https://nodejs.org 'Go to Node.js homepage')
[![PHP Version Require](http://poser.pugx.org/pugx/badge-poser/require/php)](https://packagist.org/packages/pugx/badge-poser)

 <img src="https://img.shields.io/badge/-Vue3-4FC08D?logo=vue.js&logoColor=white&style=flat"/>
 <img src="https://img.shields.io/badge/-Laravel-FF2D20?logo=laravel&logoColor=white&style=flat"/>
 <!-- <img src="https://img.shields.io/badge/-Tailwind%20CSS-06B6D4?logo=tailwind-css&logoColor=white&style=flat"/> -->
 <img src="https://img.shields.io/badge/-ViteJs-6e37a0?logo=vite&logoColor=white&style=flat"/>
 <img src="https://img.shields.io/badge/-PatternFly-004285?logo=Ghost&logoColor=white&style=flat"/>
 <img src="https://img.shields.io/badge/-mySQL-4479A1?logo=mysql&logoColor=white&style=flat"/>

</div>

<!-- Intro -->

<a name="intro"></a>

## Intro

rConfig V7 Docker Compose Setup is a set of scripts to help you get started with rConfig V7 Professional on Docker. rConfig V7 Professional is an enterprise grade Network Configuration Management (NCM) software package with superior NCM features and capabilities to help you easily manage configurations on large and small heterogenous networks. rConfig V7 Professional runs natively on many variants of Linux.

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
> rConfig V7 Professional is a paid product. You can purchase a subscription from [rConfig](https://www.rconfig.com/pricing). It Requires a valid license key to download and 
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
 docker login reg.rconfig.com:8443 -u="robot$rconfig+name"  # Enter your credentials
```
 
3. Edit the `.env` file and update the following values:

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
    php artisan passport:install
    php artisan rconfig:sync-tasks
    php artisan rconfig:sync-search 
    php artisan rconfig:clear-all
    php artisan rconfig:update-rbac-data  
    chown -R www-data:www-data /var/www/html/rconfig7/current/storage
    chmod -R 775 /var/www/html/rconfig7/current/storage
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
- [patternfly v4](https://v4-archive.patternfly.org/v4/)

See composer.json and package.json for a full list of dependencies, and their licenses.
