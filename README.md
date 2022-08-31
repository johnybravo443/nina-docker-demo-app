## demo app - developing with Docker

This demo app shows a simple user profile app set up using 
- index.html with pure js and css styles
- nodejs backend with express module
- mongodb for data storage

All components are docker-based

### With Docker

#### To start the application

Step 1: Create docker network

    docker network create mongo-network 

Step 2: start mongodb 

    docker run -d -p 27017:27017 -e MONGO_INITDB_ROOT_USERNAME=admin -e MONGO_INITDB_ROOT_PASSWORD=password --name mongodb --net mongo-network mongo    

Step 3: start mongo-express
    
    docker run -d -p 8081:8081 -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin -e ME_CONFIG_MONGODB_ADMINPASSWORD=password --net mongo-network --name mongo-express -e ME_CONFIG_MONGODB_SERVER=mongodb mongo-express   

_NOTE: creating docker-network in optional. You can start both containers in a default network. In this case, just emit `--net` flag in `docker run` command_

Step 4: open mongo-express from browser

    http://localhost:8081

Step 5: create `user-account` _db_ and `users` _collection_ in mongo-express

Step 6: Start your nodejs application locally - go to `app` directory of project 

    npm install 
    node server.js
    
Step 7: Access you nodejs application UI from browser

    http://localhost:3000

### With Docker Compose

#### To start the application

Step 1: start mongodb and mongo-express

    docker-compose -f docker-compose.yaml up -d
    
_You can access the mongo-express under localhost:8080 from your browser_
    
Step 2: in mongo-express UI - create a new database "my-db"

Step 3: in mongo-express UI - create a new collection "users" in the database "my-db"       
    
Step 4: start node server 

    npm install
    node server.js
    
Step 5: access the nodejs application from browser 

    http://localhost:3000

#### To build a docker image from the application

    docker build -t my-app:1.0 .       
    
The dot "." at the end of the command denotes location of the Dockerfile.

### With Docker Compose (when app is also dockerized)

#### To start the application

Step 1: Push the image to AWS ECR after changing the connection in server.js to _mongoUrlDocker_
and updating the connection name at line 38 and 60 to be able to run the
app as a dockerized app.

NOTE: Revert it back to _mongoUrlLocal_ if you want to run the app locally undockerized 

Step 2: Update docker-compose and add the following (see _docker-compose.yaml_)

      my-app:
        image: 489546524871.dkr.ecr.us-east-1.amazonaws.com/my-app:1.3
        ports:
         - 3000:3000

Step 3: start mongodb, mongo-express and my-app

    docker-compose -f docker-compose.yaml up -d

_You can access the mongo-express under localhost:8080 from your browser_

Step 4: in mongo-express UI - create a new database _my-db_

Step 5: in mongo-express UI - create a new collection _users_ in the database _my-db_

Step 6: access the app at

    http://localhost:3000

Step 7: access the mongodb express UI at

    http://localhost:8081