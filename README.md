[linuxserverurl]: https://linuxserver.io
[thehomerepoturl]: https://github.com/thehomerepot
[appurl]: https://wavevms.com/
[hub]: https://hub.docker.com/r/ndwalters/hanwha-wave/

Based on the amazing work by [LinuxServer.io][linuxserverurl], and [TheHomeRepot][thehomerepoturl]. 

# ndwalters/hanwha-wave

[Hanwha Wisenet Wave][appurl] is a free to view [VMS](https://en.wikipedia.org/wiki/Video_management_system) that adds recording capability with the purchase of camera licenses.

[![Hanwha Wisenet Wave](https://github.com/ndwalters/hanwha-wave/blob/latest/hanwha-wisenet-wave.png)][appurl]

## Usage

```
docker run -d \
  --name=hanwha-wave \
  --restart=unless-stopped \
  --net=host \
  -e PUID=<UID> 
  -e PGID=<GID> \
  -e TZ=<timezone> \
  -v </path/to/config>:/config \
  -v </path/to/archive>:/archive \
ndwalters/hanwha-wave:latest
```

## Parameters

The parameters are split into two halves, separated by a colon, the left hand side representing the host and the right the container side. 
For example with a port -p external:internal - what this shows is the port mapping from internal to external of the container.
So -p 8080:80 would expose port 80 from inside the container to be accessible from the host's IP on port 8080
http://192.168.x.x:8080 would show you what's running INSIDE the container on port 80.


* `--net=host` - Shares host networking with container
* `-v /config` - Configuration files
* `-v /archive` - Recordings will be landed here.
* `-e PGID=` for for GroupID - see below for explanation
* `-e PUID=` for for UserID - see below for explanation
* `-e TZ` - for timezone information *eg Europe/London, etc*

### User / Group Identifiers

Sometimes when using data volumes (`-v` flags) permissions issues can arise between the host OS and the container. We avoid this issue by allowing you to specify the user `PUID` and group `PGID`. Ensure the data volume directory on the host is owned by the same user you specify. DO NOT USE ROOT

In this instance `PUID=1001` and `PGID=1001`. To find yours use `id user` as below:

```
  $ id <dockeruser>
    uid=1001(dockeruser) gid=1001(dockergroup) groups=1001(dockergroup)
```

## Setting up the application

You will need to install and run the [Wave Desktop Client](https://wavevms.com/download/) and connect to your docker host using port 7001. You will be prompted within the client to configure the new server instance.

## Licensing

In order to record from your security cameras, you will need to purchase licenses. These are purchased PER actively recording camera and can be moved between cameras.

**However, the licenses are tied to a uniqely generated HWID (hardware ID) and cannot easily be moved. This may change in v3.1.X but currently you must log a support request to have licenses moved to a new server. For this reason be mindful of where you install licenses. I would not currently do so inside a docker container**

## Info

* Shell access whilst the container is running: `docker exec -it hanwha-wave /bin/bash`
* To monitor the logs of the container in realtime: `docker logs -f hanwha-wave`


## Versions

+ **2021.08.20:** Initial creation
