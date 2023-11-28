---
title: Yt-dlp download worker with Go, or how not to do "Hello World" with Go
date: 2023-11-28 13:04:00 Z
---

So, I decided that it would probably be a good idea to learn GoLang.
For some time, I wondered what I should do with Go.
Should I implement Levenstein distance algorithms and compare performance? - Useless, there are multiple implementations already, too academic
Should I implement MySQL UDF? - as a first project, it scares me to mix C and Go, so maybe it should be a second project.
Should I create something that downloads videos for future projects? - maybe I always liked scraping data from WEB managing media files.

# Starting my first Go project
My go-to Go IDE will be JetBrains GoLand. I can use other IDEs, but I am used to JetBrains IDEs, and I bought all IDEs ages ago.
![Screenshot from 2023-11-28 15-23-48.png](/uploads/Screenshot%20from%202023-11-28%2015-23-48.png)

I should probably try to connect to some queuing system with Go to receive messages/events (Kafka, RabitMQ, Beanstalkd). Easier would be to create a REST microservice, but from experience, that doesn't work well with long-running tasks.

First, I need "docker-compose.yml" to start the queue service. As in previous post I will use Apache Pulsar:

```yaml
version: "3.7"
services:
  pulsar:
    image: apachepulsar/pulsar:2.6.0
    command: bin/pulsar standalone
    hostname: pulsar
    ports:
      - "8081:8080"
      - "6650:6650"
    restart: unless-stopped
    volumes:
      - "./data/:/pulsar/data"
  dashboard:
    image: apachepulsar/pulsar-manager:v0.2.0
    ports:
      - "9527:9527"
      - "7750:7750"
    depends_on:
      - pulsar
    links:
      - pulsar
    environment:
      SPRING_CONFIGURATION_FILE: /pulsar-manager/pulsar-manager/application.properties
```



