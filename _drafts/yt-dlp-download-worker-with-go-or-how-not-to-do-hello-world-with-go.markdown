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

First, I need "docker-compose.yml" to start the queue service. I feel adventurous and will chose new tool, that I never used




