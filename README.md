# projector-monitoring
Homework 1.3 for Projector Highload Software Architecture course
<br>
Configs are taken from https://github.com/bcremer/docker-telegraf-influx-grafana-stack
<br>
Plugin for docker is taken from https://github.com/influxdata/telegraf/blob/master/plugins/inputs/docker/README.md

# How to run the app
1. Copy ./backend/.env.example into ./backend/.env
2. docker-compose up

# Which services are here
1. Telegraf to collect metrics and put them to InfluxDB
2. InfluxDB to store metrics data from Telegraf
3. Grafana to visualize metrics on the dashboards
4. Node.js application (using Nest.js framework) that has:
    * POST /messages endpoint to post a message with any text and store it to MongoDb and ElasticSearch
    * GET /messages/search?search="" to search for a specific message relying on ElasticSearch
    * users model and CRUD
    * message CRUD
5. MongoDB to store the data from the backend
6. ElasticSearch cluster (3 nodes) to store the data and search in text
7. Nginx as a gateway for the backend


# Tests
## Post 100.000 requests with 10 concurrency
ab -n 100000 -c 10 -p ../ab/create-message.json http://localhost:8080/messages
This is ApacheBench, Version 2.3 <$Revision: 1903618 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking localhost (be patient)
Completed 10000 requests
Completed 20000 requests
Completed 30000 requests
Completed 40000 requests
Completed 50000 requests
Completed 60000 requests
Completed 70000 requests
Completed 80000 requests
Completed 90000 requests
Completed 100000 requests
Finished 100000 requests

Server Software:        nginx/1.27.0
Server Hostname:        localhost
Server Port:            8080

Document Path:          /messages
Document Length:        158 bytes

Concurrency Level:      10
Time taken for tests:   51.823 seconds
Complete requests:      100000
Failed requests:        0
Non-2xx responses:      100000
Total transferred:      39700000 bytes
Total body sent:        25100000
HTML transferred:       15800000 bytes
Requests per second:    1929.64 [#/sec] (mean)
Time per request:       5.182 [ms] (mean)
Time per request:       0.518 [ms] (mean, across all concurrent requests)
Transfer rate:          748.11 [Kbytes/sec] received
                        472.99 kb/s sent
                        1221.10 kb/s total

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0    0   0.0      0       1
Processing:     1    5   2.0      6      24
Waiting:        1    5   2.0      6      24
Total:          1    5   2.0      6      24

Percentage of the requests served within a certain time (ms)
  50%      6
  66%      6
  75%      6
  80%      6
  90%      7
  95%      7
  98%      9
  99%     11
 100%     24 (longest request)

 --------------------------------------------------------
 ab -n 1000000 -c 50 -p ../ab/create-message.json http://localhos
t:8080/messages
This is ApacheBench, Version 2.3 <$Revision: 1903618 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking localhost (be patient)
Completed 100000 requests
Completed 200000 requests
Completed 300000 requests
Completed 400000 requests
Completed 500000 requests
Completed 600000 requests
Completed 700000 requests
Completed 800000 requests
Completed 900000 requests
Completed 1000000 requests
Finished 1000000 requests


Server Software:        nginx/1.27.0
Server Hostname:        localhost
Server Port:            8080

Document Path:          /messages
Document Length:        158 bytes

Concurrency Level:      50
Time taken for tests:   375.555 seconds
Complete requests:      1000000
Failed requests:        0
Non-2xx responses:      1000000
Total transferred:      397000000 bytes
Total body sent:        251000000
HTML transferred:       158000000 bytes
Requests per second:    2662.73 [#/sec] (mean)
Time per request:       18.778 [ms] (mean)
Time per request:       0.376 [ms] (mean, across all concurrent requests)
Transfer rate:          1032.33 [Kbytes/sec] received
                        652.68 kb/s sent
                        1685.01 kb/s total

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0    0   0.0      0       3
Processing:     4   19   7.5     20      90
Waiting:        3   19   7.5     20      90
Total:          4   19   7.5     20      90

Percentage of the requests served within a certain time (ms)
  50%     20
  66%     21
  75%     23
  80%     24
  90%     26
  95%     30
  98%     36
  99%     40
 100%     90 (longest request)