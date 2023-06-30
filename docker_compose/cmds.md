## Run the database
docker run --name mongodb --network=vidly -d -v vidly -p 27018:27017 mongo:4.0-xenial

<!-- docker run --hostname=2ef159ba229b --env=PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin --env=GOSU_VERSION=1.12 --env=JSYAML_VERSION=3.13.1 --env=MONGO_PACKAGE=mongodb-org --env=MONGO_REPO=repo.mongodb.org --env=MONGO_MAJOR=4.0 --env=MONGO_VERSION=4.0.28 --env=HOME=/data/db --volume=/data/configdb --volume=/data/db --volume=vidly --network=vidly -p 27018:27017 --restart=no --label='desktop.docker.io/wsl-distro=Ubuntu' --runtime=runc -d mongo:4.0-xenial -->

## Build and run the backend
dbname: mongodb
dbport: 27018

docker build -t vidly_back .
docker run --name vidly_back --network=vidly --env=DB_URL=mongodb://mongodb:27018/vidly -d -p 3001:3001 vidly_back

## Build and run the frontend
docker build -t vidly_front .

docker run --name vidly_front --network=vidly -d -p 3000:3000 vidly_front


---

# Now Building for production
## Frontend
docker build -t vidly_front_prod .

docker run --name vidly_front_prod --network=vidly -d -p 3000:80 vidly_front_prod

## Backend
dbname: mongodb
dbport: 27018

docker build -t vidly_back_prod -f Dockerfile.prod .
docker run --name vidly_back_prod --network=vidly --env=DB_URL=mongodb://mongodb:27018/vidly -d -p 3001:3001 vidly_back_prod