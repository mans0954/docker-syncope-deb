# docker-syncope-deb
Unofficial Docker image for Apache Syncope, from the debs

For official information about Apache Syncope, please see:

https://syncope.apache.org/

Apache Syncope is distributed under the Apache 2.0 license and is Copyright 2012-2017 The Apache Software Foundation.

The files in this project are distributed under the Apache 2.0 license and Copyright Christopher Hoskin.

THIS CONTAINER IS PURELY FOR DEMONSTRATION PURPOSES ON YOUR LOCALHOST. IT IS NOT IN ANY WAY PRODUCTION READY. IT USES WEAK DEFAULT PASSWORDS.

The container requires a separate container to provide a Postgresql database. The name of the database, the role and the password are all 'syncope'.

Note, this version does not enable Activiti due to licensing reasons.

#Use

Start postgres:

```docker run -d --name postgres-syncope -e POSTGRES_USER=syncope -e POSTGRES_PASSWORD=syncope -e POSTGRES_DB=syncope postgres```

Check that the database is up:

```docker logs postgres-syncope```

Start ConnId server:

```docker run -d --name connid-connector-server  mans0954/connid-connector-server```


Start Syncope:

```docker run -ti --link postgres-syncope:postgres --link connid-connector-server:connid-connector-server mans0954/syncope-deb```

Browse to http://172.17.0.4:8080/syncope-console/ (your IP address may be different)

Login, through basic auth, with username 'admin' and password 'password'.

Swagger is avaliable at:

http://172.17.0.4:8080/syncope/swagger/


