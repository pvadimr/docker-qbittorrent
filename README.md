What is qBittorrent?
====================

[qBittorrent](http://www.qbittorrent.org/) NoX is the headless with remote web interface version of qBittorrent BitTorrent client.

![qBittorrent logo](https://github.com/pvadimr/docker-qbittorrent/blob/master/docs/qbittorrent-logo.png?raw=true)

Based on: https://github.com/wernight/docker-qbittorrent

Usage
-----

All mounts and ports are optional and qBittorrent will work even with only:

    $ docker run pvadimr/qbittorrent

... however that way some ports used to connect to peers are not exposed, accessing the
web interface requires you to proxy port 8080, and all settings as well as downloads will
be lost if the container is removed. So start it using this command:

    $ mkdir -p config torrents downloads
	$ docker run -d --user $UID:$GID \
	-p 8080:8080 -p 6881:6881/tcp -p 6881:6881/udp \
	--name=qbittorrent --restart=always \
	-v /data/qbittorrent/config:/config \
	-v /data/qbittorrent/data:/torrents \
	-v /data/downloads:/downloads \
	pvadimr/qbittorrent

... to run as yourself and have WebUI running on [http://localhost:8080](http://localhost:8080)
(username: `admin`, password: `adminadmin`) with config in the following locations mounted:

  * `/config`: qBittorrent configuration files
  * `/torrents`: Torrent files
  * `/downloads`: Download location

Note: By default it runs as UID 520 and GID 520, but can run as any user/group.

It is probably a good idea to add `--restart=always` so the container restarts if it goes down.

You can change `6081` to some random  port number (also change in the settings).

_Note: For the container to run, the legal notice had to be automatically accepted. By running the container, you are accepting its terms. Toggle the flag in `qBittorrent.conf` to display the notice again._

_Note: `520` was chosen randomly to prevent running as root or as another known user on your system; at least until [issue #11253](https://github.com/docker/docker/pull/11253) is fixed._