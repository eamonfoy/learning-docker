


# Docker File

Sample docker file for node project

```yml
FROM node:alpine

WORKDIR '/app'

COPY package.json .

RUN npm install
COPY . .

CMD ["npm","start"]
```

# Sample Compose file

Docker compose file sample
```yml
version: '3'

services:
    redis-server:
        image: 'redis'
    node-app:
        build: .
        ports:
            - "4001:8081"

```

# Extract of a node app calling redis

Connection a node server to a redis server within docker
```javascript

const app = express();
const client = redis.createClient({
    host: 'redis-server',
    port: 6379
});

```

# Using Docker compose
Getting docker compose to build the container

```bash
docker-compose up --build

```

# Running Docker containers in background mode

run in back ground mode
```
docker run -d redus
```

get running containers
```
docker ps
```

stop a running container
```
docker stop <container id>
```


# Starting and stopping docker-compose

Start compose in backround
```
docker-compose up -d
```

check containers running by running docker ps
```
dcoker ps
```

Stoping compose
```
docker-compose down
```


# Mitigating containers failing by using the restart 'always' command

```yml
version: '3'

services:
    redis-server:
        restart: always
        image: 'redis'
    node-app:
        restart: always
        build: .
        ports:
            - "4001:8081"

```

# Status code for process exist

0 = we exited and everything is OK   
1,2,3 etc  = we existed because something went wrong!

# Using a pipeline to deploy to production
 
 ```
 ---development --> testing --> production --|   
 |----------------<<<------------------------|   
 ```





Generqate empty react app for testing

```
npm install -g create-react-app
create-react-app frontend
```

# npm usefull comannds

Starts a development server for development purpose only
```
npm run start
```
Runs tests associated with the project
```
npm run test
```
Builds a production version of the application
```
npm run build
```


# Development docker file

used only for development
dockerfile.dev
```yml
FROM node:alpine

WORKDIR '/app'

COPY package.json .

RUN npm install
COPY . .

CMD ["npm","run","start"]
```

Building the development docker file
```
docker build -f dockerfile.dev .
```

Running the container
```
docker run -p 3000:3000 -v /app/node_modules -v $(pwd):/app <image id>

note:
 -v /app/node_modules 
 this means use the /app/node_modules contained within the container instaead of the app directory
 this is a placeholder for a folder in the container
```


if you are running on Windows, please read this: Create-React-App has some issues detecting when files get changed on Windows based machines.  To fix this, please do the following:

    In the root project directory, create a file called .env

    Add the following text to the file and save it: CHOKIDAR_USEPOLLING=true

    That's all!

For more on why this is required, you can check out: https://facebook.github.io/create-react-app/docs/troubleshooting#npm-start-doesn-t-detect-changes


Doing the same with docker compose
```yml
version: '3'
services:
    web:
        build: 
            context: .
            dockerfile: Dockerfile.dev
        ports:
            - "3000:3000"
        volumes:
            - /app/node_modules
            - .:/app

```


# Running the development tests within a container

```
docker run -it <imnage id> npm run test
```

# Add a test service for testing only

```yml
version: '3'
services:
    web:
        build: 
            context: .
            dockerfile: Dockerfile.dev
        ports:
            - "3000:3000"
        volumes:
            - /app/node_modules
            - .:/app
    tests:
        build: 
            context: .
            dockerfile: Dockerfile.dev
        volumes:
            - /app/node_modules
            - .:/app
        command: ["npm", "run", "test"]


   
```





















