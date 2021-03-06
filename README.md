nginxify
========

Nginxify parses JSON configurations of NGINX config files and creates the appropriate representations in configuration.

## NGINX Config File Location

The default generated configuration file is located at `/etc/nginx/sites-enabled/nginxify.conf`, if this is not included by default, for example on Mac OS X, you will need to include this file in your `nginx.conf` file using the [include](http://nginx.org/en/docs/ngx_core_module.html#include) directive.

## Usage

Create a file called `.nginx` in your user home directory (`/home/username/.nginx` or `/Users/username/.nginx` on Mac OS X), and populate according to the following JSON. Each user can have their own `.nginx` file, and they will be merged in the order returned by the `glob` package in Python.

Be sure to [install NGINX](http://wiki.nginx.org/Install) before continuing.

**Important:** All paths are relative to the home directory containing the `.nginx` file, as such do not begin paths with a `/` or `./` - all projects should be under your home directory. You can use projects that are not within your home directory, but those must be proxied to a local port.

```js
{
  "my.domain.com": {
    "paths": {
      "/": "projects/my_domain/public", // this is a directory relative to your home directory,
      "/testapp": 3000  // This will proxy all requests to a server running on port 3000
    }
  },

  "other.domain.com": {
    "paths": {
      "/foo": "another/folder/path"
    }
  },

  "another.domain.com": {
    "paths": {
      "/": ["sphp", "path/to/sphp-project"]
    }
  },

  "php.domain.com": {
    "paths": {
      "/": ["php", "path/to/php-project"]
    }
  }
}
```
#### Static

Static site path config has a string path relative to your home folder.

#### Proxy Pass

Proxy pass to a port with an integer port number as the path config.

#### PHP

PHP using [PHP-FPM](http://php-fpm.org/) is supported with the path config `["php", "path/to/project"]`.

**Important:** You must configure `php-fpm.conf` to listen on a unix socket with the following line: `listen = /var/run/php5-fpm.sock`.

#### SimplifiedPHP

[SimplifiedPHP](https://github.com/NateFerrero/simplified-php) using [PHP-FPM](http://php-fpm.org/) is supported with the path config `["sphp", "path/to/project"]`.

**Important:** You must configure `php-fpm.conf` to listen on a unix socket with the following line: `listen = /var/run/php5-fpm.sock`.

**Important:** You must install the `simplified-php` project at `/usr/share/simplified-php`, this can be confirmed by running `cat /usr/share/simplified-php/simplified.php` - if the file is displayed, you should be good to go.

## Running Nginxify

Every time you change your `.nginx` file, you must run Nginxify to update the generated NGINX configuration file.

#### Linux

On Linux, when you have completed editing the configuration file, and after nginxify has been installed (see below), run the `nginxify` command in a shell.

#### Mac OS X

On Mac OS X, run `sudo python /bin/nginxify && sudo nginx -s reload` to regenerate the configuration file.

## Installation - Mac OS X

```bash
git clone git@github.com:NateFerrero/nginxify.git
cd nginxify
sudo make install-mac-os-x
```

For PHP, be sure to fix permissions for /var/run socket.

## Installation - Linux

```bash
git clone git@github.com:NateFerrero/nginxify.git
cd nginxify
sudo make install
```

## Allowing users to run `nginxify` without sudo

Add the following to /etc/sudoers using `visudo`:

```bash
# Nginxify
%nginxify ALL=NOPASSWD: /bin/nginxify, /etc/init.d/nginx reload
```

Add a user to the nginxify group:

```bash
sudo usermod -a -G nginxify the_username
```

Keep in mind that the user will have to open a new shell to use the new group permissions.
