Docker image for WordPress development

# Features

Image based on the official WordPress image and packed with all the needed WordPress development tools, including:

* Xdebug
* WP-CLI
* Mailhog

# Usage

Pull the image: `docker pull bracketspace/wordpress`

WP-CLI command in docker-compose: `docker-compose exec --user www-data wordpress wp --version`

Mailhog available on: `http://localhost:8025`

# Available PHP versions:

* PHP 7.2 - `bracketspace/wordpress:php7.2`
* PHP 7.1 - `bracketspace/wordpress:php7.1`
* PHP 7.0 - `bracketspace/wordpress:php7.0`
* PHP 5.6 - `bracketspace/wordpress:php5.6`
