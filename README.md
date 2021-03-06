# nodejs-mongo-test

# First : mongodb을 시작한다. ( 여기선 docker를 이용하여 시작 ). 
--------------------

*몽고디비 컨테이너에 생성*

<pre><code>$ docker run --name mongo -p 27017:27017 -d mongo</code></pre>

*도커 프로세스 확인*
<pre><code>$ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                      NAMES
b2482f6a58be        mongo               "docker-entrypoint.s…"   5 minutes ago       Up 5 minutes        0.0.0.0:27017->27017/tcp   mongo</code></pre>


*컨테이너에 접속*
<pre><code>$ docker exec -it mongo /bin/bash</code></pre>

*mongo를 입력하여 제대로 동작하는지 확인*
<pre><code>root@b2482f6a58be:/# mongo

MongoDB shell version v4.0.0
connecting to: mongodb://127.0.0.1:27017
MongoDB server version: 4.0.0
Server has startup warnings:
2018-06-30T04:51:49.681+0000 I STORAGE  [initandlisten]
2018-06-30T04:51:49.681+0000 I STORAGE  [initandlisten] ** WARNING: Using the XFS filesystem is strongly recommended with the WiredTiger storage engine
2018-06-30T04:51:49.681+0000 I STORAGE  [initandlisten] **          See http://dochub.mongodb.org/core/prodnotes-filesystem
2018-06-30T04:51:50.154+0000 I CONTROL  [initandlisten]
2018-06-30T04:51:50.154+0000 I CONTROL  [initandlisten] ** WARNING: Access control is not enabled for the database.
2018-06-30T04:51:50.154+0000 I CONTROL  [initandlisten] **          Read and write access to data and configuration is unrestricted.
2018-06-30T04:51:50.154+0000 I CONTROL  [initandlisten]
---
Enable MongoDB's free cloud-based monitoring service to collect and display
metrics about your deployment (disk utilization, CPU, operation statistics,
etc).

The monitoring data will be available on a MongoDB website with a unique
URL created for you. Anyone you share the URL with will also be able to
view this page. MongoDB may use this information to make product
improvements and to suggest MongoDB products and deployment options to you.

To enable free monitoring, run the following command:
db.enableFreeMonitoring()
>

>  db
test
</code></pre>

# Second : nodejs 실행
-------------------
*우선 해당 git에 프로젝트 다운*
<pre><code>$ git clone https://github.com/yangjeehan/nodejs-mongo-test.git </pre></code>

*해당 도커파일을 통해 이미지르 만든다.*
<pre><code>$ docker build -t nodejs-mongodb-test:yang</pre></code>

*만든 이미지를 실행한다. ( 전에 만든 mongo container을 link을 통해 연결 ) *
<pre><code>$ docker run -i --link mongo -t nodejs-mongodb-test:yang /bin/bash </pre></code>

*만든 컨테이너에 접속하여 아래명령어 수행*
<pre><code>$ docker exec -it ee137a79dd4e /bin/bash 
root@ee137a79dd4e:/usr/src/app# node demo_mongo_insert.js </pre></code> 

# 몽고디비에 접속해서 데이터 입력 체크 
~~~
root@b2482f6a58be:/# mongo

> show databases;
admin   0.000GB
config  0.000GB
local   0.000GB
mydb    0.000GB

> use mydb
switched to db mydb
>
> show collections;
customers

> db.customers.find()
{ "_id" : ObjectId("5b371eb7d8daba00078e2fdf"), "name" : "Company Inc", "address" : "Highway 37" }

~~~

# End 

