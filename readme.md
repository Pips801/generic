# Game Download Cache Docker Container

## Introduction

This docker container provides a caching proxy server for game download content. For any network with more than one PC gamer in connected this will drastically reduce internet bandwidth consumption. 

The primary use case is gaming events, such as LAN parties, which need to be able to cope with hundreds or thousands of computers receiving an unannounced patch - without spending a fortune on internet connectivity. Other uses include smaller networks, such as Internet Cafes and home networks, where the new games are regularly installed on multiple computers; or multiple independent operating systems on the same computer.

This container is designed to support any game that uses HTTP and also supports HTTP range requests (used by Origin). This should make it suitable for:

 - Origin (EA Games)
 - Riot Games (League of Legends)
 - Battle.net (Hearthstone, Starcraft 2, Overwatch)
 - Frontier Launchpad (Elite Dangerous, Planet Coaster)
 - Uplay (Ubisoft)
 - Windows Updates

You can use this container for Steam, however [steamcache container](https://hub.docker.com/r/steamcache/steamcache/) is a better option as it uses a different caching method that's better for Steam.

## Usage

You will need to have a DNS server forwarding queries to the machine your docker container is running on. You can use the [steamcache-dns](https://hub.docker.com/r/pips/steamcache-dns/) docker image to do this or you can use a DNS service already on your network see the [steamcache-dns github page](https://github.com/steamcache/steamcache-dns) for more information.

Run the origin container using the following to allow TCP port 80 (HTTP) through the host machine:

```
docker run --name lancache -p <ip>:80:80 pips/lancache:latest
```

*Hey!:* you will need to add a second IP to the interface you run the lancache on. More info [here](https://www.garron.me/en/linux/add-secondary-ip-linux.html)

## Quick Explanation

For an game cache to function on your network you need two services.

* A game cache service [This container](https://github.com/Pips801/lancache)
* A steam cache service [steamcache](https://github.com/steamcache/steamcache)
* A special DNS service [steamcache-dns](https://github.com/Pips801/steamcache-dns)

The cache service transparently proxies your requests for content to the content procider, or serves the content to you if it already has it.

The special DNS service handles DNS queries normally (recursively), except when they're about the games you're interested in and in that case it responds that the depot cache service should be used.

## Monitoring

Tail the access logs (default: /data/logs/access.log).

```
docker exec lancache tail -f /data/logs/access.log)
```

## Running on Startup

Follow the instructions in the Docker documentation to run the container at startup.
[Documentation](https://docs.docker.com/articles/host_integration/)

## Thanks

Based on original configs from [ansible-lanparty](https://github.com/ti-mo/ansible-lanparty).
Modified from [Steamcache Team](http://lancache.net/).
