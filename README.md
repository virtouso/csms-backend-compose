
# csms-backend-compose

this project is created to provide backend infera for backend services of csms
first make sure that already clone csms-backend-authentication and csms-backend-transaction git repositories beside this folder.
when in this directory just call the command bellow:

# How TO Run Whole System?

```sh
# Start Shared Infrastructure
docker network create --driver bridge shared_network

docker-compose up -d

# Navigate to Authentication Service and start it
cd ../csms-backend-authentication
docker-compose up -d

# Navigate to Transaction Service and start it
cd ../csms-backend-transaction
docker-compose up -d

cd ../csms-backend-compose

```

# why multiple single module repos?

* simpler ci/cd configs
* shared code changes can cause problem
* microservices may develop by different people and teams.
* seperation of concerns instead of big complicated monorepo


# swagger support

*to create default stations and drivers on the authentication service you can call create methods.
* also transaction service api is accessible through swagger. it only works in dev mode.

* http://localhost:60602/swagger-ui/index.html#  -- transaction
* http://localhost:60601/swagger-ui/index.html#/ -- authentication


# how it works?

in general there is single rest http api. no need top open sockets or live connection.

there is a correlation  id between to microservices to sync kafka request and response. everything handled by the libray. there is timeout for requests and keeping state of the correlation is done by the library. so no map(dictionary) or redis caching needed. 

reading configs from properties and environment as much as possible. 

infera owned by each servics like postgress database are implemented in service docker-compose.yml while general infera like kafka ins implemented in seperate infera docker-compose.yml

global exception handling and general metaresult for better debugging for client sides added. 
did not use many best practices like ddd and tdd as the test was mostly concetrated on kafka communication but it was possible to work on them.
using lombok could help have more encapsulated classes with getters and setters but had some library resolving problems based on bad internet that made me ignore it. 