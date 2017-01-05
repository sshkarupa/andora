# Andora

Andora is ansible roles to deploy Ruby on Rails applications with docker containers in a capistrano style

Project name is [an]sible, [do]cker and [ra]ils

## Features

- Setup a host with dependencies and structure of directories
- Create a swapfile if needed
- Deploy a release
- Export database data
- Rollback to the previous release

## Requirements

- Docker in your deployer machine

## Usage

Install andora

```
$ cd project
$ git clone git@github.com:weazar/andora.git ansible
```

Add to `.ssh/config`

```
Host *
  ForwardAgent yes
```

Run an ansible docker container

```
$ cd ansible
$ docker run -v ~/.ssh:/root/.ssh -v "$PWD":/mnt/src --rm -it weazar/ansible /bin/bash
```

Set hosts in [inventory.ini](inventory.ini)

Set variables in [group_vars/webservers](group_vars/webservers)

- Set your app name
- Set git repository path

Set environment variables in [playbooks/roles/deploy/templates/env.j2](playbooks/roles/deploy/templates/env.j2)

Add env.j2 to .gitignore

Setup a host

```
$ ansible-playbook playbooks/setup.yml
```

Create swapfile

```
$ ansible-playbook playbooks/swap.yml
```

Deploy release

```
$ ansible-playbook playbooks/deploy.yml
```

Deploy release without new gems, migrates and assets

```
$ ansible-playbook playbooks/deploy.yml -t fast
```

Export database data

- Put your dump file to `playbooks/roles/export_data/files/structure.sql`

```
$ ansible-playbook playbooks/export_data.yml
```

Rollback to the previous release

```
$ ansible-playbook playbooks/rollback.yml
```

Restart all services

```
$ ansible-playbook playbooks/restart.yml
```

**How to configure docker for rails application see [weazar/dora](https://github.com/weazar/dora)**

**Works fine with [DigitalOcean](https://m.do.co/c/fcd1d1654add)**

## License

Author: Yuri Smirnov <hello@yurismirnov.com>

Licensed under the [MIT License](http://www.opensource.org/licenses/MIT).
