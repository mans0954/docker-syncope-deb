# docker-syncope-deb
Unofficial Docker image for Apache Syncope, from the debs

For official information about Apache Syncope, please see:

https://syncope.apache.org/

Apache Syncope is distributed under the Apache 2.0 license and is Copyright 2012-2017 The Apache Software Foundation.

The files in this project are distributed under the Apache 2.0 license and Copyright Christopher Hoskin.

THIS CONTAINER IS PURELY FOR DEMONSTRATION PURPOSES ON YOUR LOCALHOST. IT IS NOT IN ANY WAY PRODUCTION READY. IT USES WEAK DEFAULT PASSWORDS.

The container requires a separate container to provide a Postgresql database. The name of the database, the role and the password are all 'syncope'.

Note, this version does not enable Activiti due to licensing reasons.

# Use - docker-compose

docker-compose up

# User - manual

Start postgres:

```docker run -d --name postgres-syncope -e POSTGRES_USER=syncope -e POSTGRES_PASSWORD=syncope -e POSTGRES_DB=syncope postgres```

Check that the database is up:

```docker logs postgres-syncope```

Start ConnId server:

```docker run -d --name connid-connector-server  mans0954/connid-connector-server```


Start Syncope:

```docker run -ti --link postgres-syncope:postgres --link connid-connector-server:connid-connector-server --name syncope mans0954/syncope-deb```

Browse to http://syncope.docker:8080/syncope-console/ (your IP address may be different)

Login, through basic auth, with username 'admin' and password 'password'.

Swagger is avaliable at:

http://syncope.docker:8080/syncope/swagger/

# ConnID setup

* In the admin console, click on 'Configuration'

* Select 'Policies'

* Click on the 'Account' tab and create a new Account Policy called 'Default Account Policy'

* Click on the 'Password' tab and create a new Password Policy called 'Defualt Password Policy'

* Click on the 'Pull' tab and create a new Pull Policy called 'Default Pull Policy'


* In the admin console, click on 'Topology'

* Click on the ConnID connector

* Click 'Add New Connector'

* Displayname: 'Test CSV Connector'

* Bundle: net.tirasa.connid.bundles.csvdir

* Version: 0.8.5

* Next

* Source Path: /opt/connid-connector-server/test-files/

* File mask: test-file.*.csv

* Key column name: id

* Password column name: password

* Column names: id, username, password, email

* Next

* >>

* Finish

* Click on 'Test CSV Connector'

* Select 'Add new resource'

* Key 'Test CSV Resource'

* Next

* Next

* Chose the Default Account, Password and Pull Policies created above

* Finish

* Click on 'Test CSV Resource'

* Click on 'Edit provision rules'

* Click '+'

* Type:USER

* Next

* BaseGroup >

* Add username, username, Remote Key

* Add password, password, Password

* Add email, email, email

* Next, Finish, Save

* Select 'Test CSV Resource'

* Click 'Explore Resource'

* Verify that the users can be seen

* Click on a user and verify that their username and email has been set

* Log out and log in as a user to verify that their password has been set.

