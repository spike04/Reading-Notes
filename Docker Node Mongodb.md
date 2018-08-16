# Docker - Node with Mongo
Here we will create a container for nodejs with mongo as well.

- Create a sample Express Project with Mongo connection.
- Replace localhost or 127.0.0.1 with mongo - we will be referencing to mongo container later
- Create a Dockerfile

```
FROM node:10

WORKDIR /usr/src/app

COPY package*.json ./ 	- copy package.json and package-									lock.json to WORKDIR
RUN npm install

COPY . .					- Copy all files into WORKDIR

EXPOSE 3000				- Expose PORT 3000

CMD ["npm, "start"]		- run command npm start
```

Now to run both using **Docker Compose**

## Creating Docker Compose
Create a file named docker-compose.yml with following commands
```yaml
version: '3'
services:
  app:
    container_name: docker-node-mongo
    restart: always		- if crashed, restart
    build: .
    ports:
      - '80:3000'			- mapping port
    links:
      - mongo				- Linking to mongo service
  mongo:
    container_name: mongo
    image: mongo
    ports:
      - '27017:27017'
```

Note: if you have your local mongo running on port 27017, then change the following:
```yaml
...
mongo:
    ...
    ports:
        - 27018:27017
```

And in the application where we connect to mongodb. Change to 
```javascript
mongoose.connect(
	'mongodb://mongo:27018:/<db_name>'
...
```

### Now to run the container

`docker-compose up` - Read docker-compose.yml file and create a container

Now to run the mongo in background do

`docker-compose up -d` - Run in background in detach mode

### Stop containers

`docker-compose down` - This will stop containers in the system