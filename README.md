scratch-roo-webapp
==============

A very simple webapp that can be used to try out Spring Roo.

Because of the nature of Spring Roo there is no actual Java code in this project, there is just a simple Roo script.

To run this project you will first have to [install Spring Roo](http://static.springsource.org/spring-roo/reference/html/intro.html#intro-installation "Install Spring Roo").
* One thing to note about the installation. If you have installed Spring Roo into your `/opt` directory as `root` it will not start up correctly. This is because it tries to create a `/cache` directory in the `$ROO_HOME` directory which can only be written to by `root`. So to fix this you will need to edit the `$ROO_HOME/bin/roo.sh` file and edit the following line as follows.
    Change:
    ROO_OSGI_FRAMEWORK_STORAGE="$ROO_HOME/cache"
    To:
    ROO_OSGI_FRAMEWORK_STORAGE="$HOME/.roo/cache"

Spring Roo requires Apache Maven to be installed as well so if you haven't already [install Maven](http://maven.apache.org/download.cgi "Install Maven").

Once the installation is complete navigate to the projects directory, start up Spring Roo, and run the Roo script.

    $ cd scratch-roo-webapp
    $ roo.sh
    roo> scriscript --file scratch-roo-webapp.roo
    ...
    roo> quit

The project is now ready to run with either the tomcat or jetty plugins.

    $ mvn tomcat:run

    $ mvn jetty:run

When the webapp is running it is possible to carry out crud operations on simple users with the following REST calls.

###### Create
    $ curl -v -XPOST -H "Content-Type:application/json" http://localhost:8080/scratch-roo-webapp/users -d '{
        "email": "some.one@there.com",
        "firstName": "Some",
        "lastName": "One"
    }'

###### Retrieve
    $ curl -XGET -H "Accept:application/json" http://localhost:8080/scratch-jee-webapp/scratch/users
    $ curl -XGET -H "Accept:application/json" http://localhost:8080/scratch-jee-webapp/scratch/users/1

###### Update
Notice that the `"version": 0` attribute, this must be set to be equal to the current version otherwise the update will fail.

    $ curl -v -XPUT -H "Content-Type:application/json" http://localhost:8080/webapp/users/1 -d '{
        "email": "some.one@there.com",
        "firstName": "Some",
        "lastName": "Two",
        "version": 0
    }'

###### Delete
    $ curl -XDELETE -H "Accept:application/json" http://localhost:8080/scratch-jee-webapp/scratch/users/1
