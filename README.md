nginxify
========

Nginxify parses JSON configurations of NGINX config files and creates the appropriate representations in configuration.

## Installation

```bash
git clone git@github.com:NateFerrero/nginxify.git
cd nginxify
sudo make install
```

## Allowing Users to run `nginxify`

```bash
sudo usermod -a -G nginxify the_username
```

Keep in mind that the user will have to open a new shell to gain the new group permissions.