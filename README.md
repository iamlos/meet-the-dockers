# meet-the-dockers

#1. docker-machine
    docker-machine ls
    docker-machine create 
    docker-machine create --driver virtualbox -h
    docker-machine create playground --driver=virtualbox
    docker-machine ls
    docker-machine ssh
    //Now docker is connecting to the socket, not tcp. Check for DOCKER_HOST, it's not set because it doesn't need to be
    
    exit
    
    //View the ip of the VM
    docker-machine ip playground
    
    //Print environment variables that allow you to connect docker binary to docker daemon
    docker-machine env playground
    
    //Set the environment variables to allow you to connect to docker daemon
    eval "$(docker-machine env playground)"
    docker ps
    
    docker-machine stop playground
    docker-machine start playground
    docker-machine stop playground
    docker-machine rm playground
    docker-machine ls
    
    //when stopping and starting the vm, you may be asked to run
    docker-machine regenerate-certs
    
    //Shows help
    docker-machine
#2. docker
    //Run or open the quickstart terminal if you haven't already done so
    eval "$(docker-machine env playground)"
    docker run -d -p 8000:80 nginx
    docker logs
    docker rm
    docker rmi
    docker run
    docker top
    
    docker inspect container_name
    
    docker ps
    docker info
    
    # View downloaded images
    docker images
    
    docker pull busybox
    
    docker push
    docker create
    docker start
#3. docker-compose and wordpress
    https://hub.docker.com/_/wordpress/
    
    docker pull wordpress (optional)
    docker-compose up
    http://localhost:8000
    //Doesn't work!?
    
    //get ip of host
    docker env default
    
    //ip will be something like 192.168.99.100
    http://host-ip:8080docker hub
        
#4. whalesay/docker hub
    docker run docker/whalesay cowsay any text you want here
    run this a few times with different messages (for FUN!()
    docker ps
    docker ps -a
    docker rm name-of-container
    docker rm $(docker ps -aq)
#5. bash
    docker run -it --name busybox busybox
    mkdir anyDirName
    exit
    docker ps -a
    docker commit 
#6. dockerfile
    make a simple Dockerfile
#7. gcm
    https://github.com/googlesamples/gcm-playground.git
#8. cc-reader-ordering-service
    https://github.com/infusionsoft/cc-reader-ordering-service
#9. flagship-mysql
    meet-the-dockers branch
#10. ephemeral
    docker run -rm
    https://github.com/ryanwalker/svn-to-git-dockerized
#11. cas
    delmar started this
    Everybody try it
    Setup CAS Locally steps:
    1. git clone https://github.com/infusionsoft/infusionsoft-cas.git
    2. mvn clean install (note: Use JDK 7)
    3. /etc/host file - 127.0.0.1    devcas.infusiontest.com
    4. mysql createdb -u eric -p cas
    5. run cas server using "mvn tomcat6:run-war"
    6. If needed, do temporary hack to get around know DB update error:
        a. Check the log for this error: "org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'flyway' defined in ServletContext resource [/WEB-INF/spring-configuration/infusionsoft-data.xml]: Invocation of init method failed; nested exception is com.googlecode.flyway.core.api.FlywayException: Migration to version 12 failed! Please restore backups and roll back database and code!"
        b. If it is there, then you need to do the hack. This error is a known issue where the db script, "V12__Drop_LoginAttempt_Success_Column.sql", doesn't complete successfully if it is a new database.
    7. Go to MYSQL cas database and apply V12 update manually. Run this SQL: "ALTER TABLE cas.login_attempt DROP COLUMN success;"
    8. Then run this SQL:
       UPDATE cas.schema_version
       SET success = 1
       WHERE script like 'V12__Drop_LoginAttempt_Success_Column.sql';
    9. Verify that script now shows success = 1 by running this SQL:
       SELECT *
       FROM cas.schema_version
       WHERE script like 'V12__Drop_LoginAttempt_Success_Column.sql';
    10. After fixing hack, then rerun "mvn tomcat6:run-war"
    11. Modify cas database: Add to user table e.g. INSERT INTO cas.user VALUES (1, 1, 'Chad', 'Cotter', null, null, 'chad.cotter@infusionsoft.com');
    12. Modify cas database: Add to user_authority e.g. INSERT INTO cas.user_authority VALUES (1,2);
    13. login https://devcas.infusiontest.com:7443
    14. Need to set password for CAS, click "Forgot Password" link. The recover code will be in the database (SELECT * FROM cas.user). Enter recover code and set new password.
    15. (optional) Add cas role for content publishing: ROLE_CAS_LISTING_PUBLISHER_MARKETPLACE
    16. Go to flagship database. Run Select * from Contact. Make sure that you have an admin username that matches the cas username above (the username must be an email address. If it doesn't match, update Flagship database to match cas username).
    17. Run flagship: mvn tomcat6:run -pl webapp -P cas
    18. check roles and make sure have cas publishing role: https://infusionsoft.infusiontest.com:8443/app/authentication/whoAmI.jsp
    19. Make sure the JDK has the valid certificates. If not, install the certs.
#12. dirty data set
    Create a dataset with data that we know has caused problems for our software.
    Anyone can easily spin this up and test against it.
    Caveat - Most database images set the data path as a data volume, which is not persisten. This can be changed though.