---
title: First time using Apache Pulsar
date: 2023-11-28 13:46:00 Z
---

Yesterday, I was in a job interview, and they asked me if I ever used Apache Pulsar.
After a short Google search, it looks like Kafka on drugs.
It's time to do a quick run with Docker and check its performance.

# Docker setup

Well, because I want pulsar with the manager, I used a sample docker-compose.yml file:

```yaml

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