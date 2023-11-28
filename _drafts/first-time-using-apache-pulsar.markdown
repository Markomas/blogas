---
title: First time using Apache Pulsar
date: 2023-11-28 13:46:00 Z
---

Yesterday, I was in a job interview, and they asked me if I ever used Apache Pulsar.
After a short Google search, it looks like Kafka on drugs.
It's time to do a quick run with Docker and check its performance.

# Docker setup

Well, because I want pulsar with the manager, I used a sample docker-compose.yml file:

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

Just run ```docker-compose up```

Then on local box run this command to create user and password:

    CSRF_TOKEN=$(curl http://localhost:7750/pulsar-manager/csrf-token)
    curl \
       -H 'X-XSRF-TOKEN: $CSRF_TOKEN' \
       -H 'Cookie: XSRF-TOKEN=$CSRF_TOKEN;' \
       -H "Content-Type: application/json" \
       -X PUT http://localhost:7750/pulsar-manager/users/superuser \
       -d '{"name": "admin", "password": "apachepulsar", "description": "test", "email": "username@test.org"}'