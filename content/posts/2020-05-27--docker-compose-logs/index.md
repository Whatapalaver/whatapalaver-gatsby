---
title: Viewing Docker-Compose Logs
subTitle: How to view specific logs for docker-compose apps, either when running in detached mode or just to give you control when stdout is whizzy past you like a crazed robot on speed.
cover: ./richard-sagredo-ZC2PWF4jTHc-unsplash.jpg

categories: ["tips"]
tags: ["tail", "docker-compose", "logs"]
---

I'm running a series of apps (or services) using docker-compose. There's the elastic stack of elastic, kibana and logstash, then a user interface and finally I've got an api sitting between the UI and elastic.

I get them all up and running with `docker-compose up` and from then on the steady stream of output churns its way through my terminal window.

I don't mind the logs taking over my terminal (I can always open another for passing commands) but the elastic stack (ELK) kicks out reams of stuff to stdout. If I'm specifically trying to debug a bit of code in my api service, any print statement there can very quickly get swamped by ELK printout.

This is where `docker-compose logs` comes in handy. 
Running that command without any clarifying flags will just output all of the logs. Not quite what I want.

## Docker Compose logs - options

```
Usage: logs [options] [SERVICE...]
Options:
    --no-color          Produce monochrome output.
    -f, --follow        Follow log output.
    -t, --timestamps    Show timestamps.
    --tail="all"        Number of lines to show from the end of the logs
                        for each container.
```

So I am just interested in the logs from my 'api' service, so I will follow the log, outputting the last 1000 lines to stdout:

`docker-compose logs --tail 1000 --follow api `

## Running Docker Compose in Detached Mode

The default mode is to start your container in the foreground with all output logged to stdout (terminal window). Now you know how to access whichever logs you are interested in, you may choose instead to use the `-d` flag and run the container in detached mode, which doesn't take over your terminal.

`docker-compose up -d`

To stop the running container you would need to remember `docker-compose stop`
