# Library-docker

This is full-featured lightweight book collection management service, based on Calibre and inotify.
It uses a patched version of Calibre and **doesn't copy your files inside the database**.


## Features

- Special [lightweight console version of Calibre](https://github.com/artiomn/calibre), which doesn't copy your files, but only creates symlinks.
- A script to monitor the library directory and start Calibre database syncronization, when you complete to add book series.
- Very convenient [Calibre-Web](https://github.com/janeczku/calibre-web) interface as a [Docker-image from the Linuxserver.io](https://github.com/linuxserver/docker-calibre-web).


## Requirements

You need to install [Docker](https://www.docker.com/) and [docker-compose](https://docs.docker.com/compose/) on your server.


## How to install

Your can install this service in 3 steps:

- Clone this repository or download as a zip file:
  `git clone --depth=1 https://github.com/artiomn/library-docker`
- Edit `docker-compose.yml` file to configure the service for the your environment.
  You just need to change a few variables, which are described below.
- Launch the service, by the running command inside the directory with `docker-compose.yml`:
  `docker-compose up -d`

After completing these steps, you will see the web-interface on port `8083`.

Variables need to be changed:

- `BOOKS_DIRECTORY` - path to the books directory.
- `DATABASE_PATH` - path to the Calibre database.
- `CONFIG_PATH` - path to the Calibre-Web configuration.

Additional, not mandatory variables:

- `IGNORE_PATTERN` - ignore glob expression passed to `calibredb --ignore=`.
- `ADD_PATTERN` - add glob expression passed to `calibredb --add=`.
- `PUID/PGID` - user and group ids.
- `TZ` - timezone.

See [Linuxserver.io documentation](https://github.com/linuxserver/docker-calibre-web#parameters).


## Troubleshooting

You can see logs, using command `docker-compose logs -f`.
If you get an an error `Failed to watch /var/log/messages; upper limit on inotify watches reached!`, try to increase max watches:

```shell
sudo sysctl fs.inotify.max_user_watches=524288
```

To persist changes:

```shell
sudo echo "fs.inotify.max_user_watches = 524288" > /etc/sysctl.d/50-inotify-max-watches.conf
```


