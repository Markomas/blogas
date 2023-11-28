---
title: First time using Apache Pulsar (Pulsar Manager UI setup)
date: 2023-11-28 13:46:00 Z
---

Yesterday, I was in a job interview, and they asked me if I ever used Apache Pulsar.
After a short Google search, it looks like Kafka on drugs.
It's time to do a quick run with Docker and check its performance.

# Docker setup

Well, because I want pulsar with the manager, I used a sample docker-compose.yml file:

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

Just run ```docker-compose up```

Then on local box run this command to create user and password:

```shell
    CSRF_TOKEN=$(curl http://localhost:7750/pulsar-manager/csrf-token)
    curl \
       -H 'X-XSRF-TOKEN: $CSRF_TOKEN' \
       -H 'Cookie: XSRF-TOKEN=$CSRF_TOKEN;' \
       -H "Content-Type: application/json" \
       -X PUT http://localhost:7750/pulsar-manager/users/superuser \
       -d '{"name": "admin", "password": "apachepulsar", "description": "test", "email": "username@test.org"}'
```

Then in the browser just go to: [http://localhost:9527/#/environments](http://localhost:9527/#/environments)
And we are greeted with the login page:
![Screenshot from 2023-11-28 17-42-28.png](/uploads/Screenshot%20from%202023-11-28%2017-42-28.png)

User: **admin**
Password: **apachepulsar**

In this window, we can add a new environment (Pulsar Broker):
![Screenshot from 2023-11-28 17-55-41.png](/uploads/Screenshot%20from%202023-11-28%2017-55-41.png)

It seems to be working... Useless, but working...








