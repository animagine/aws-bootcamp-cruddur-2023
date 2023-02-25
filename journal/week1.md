# Week 1 — App Containerization

##contenerize the backend

1.  Containerize the Backend and ensure ports are open for port 4567

```
cd backend-flask
export FRONTEND_URL="*"
export BACKEND_URL="*"
python3 -m flask run --host=0.0.0.0 --port=4567
cd ..

```

Append /api/activities/home to the url which shoul return json data

<img width="1680" alt="Screenshot 2023-02-20 at 5 14 31 PM" src="https://user-images.githubusercontent.com/1076924/221263598-8d8abbf1-e600-4415-adb1-7388b34cd66b.png">


2.  Create a Dockerfile in the backend-flask repository (backend-flaask/Dockerfile)

```
FROM python:3.10-slim-buster

WORKDIR /backend-flask

COPY requirements.txt requirements.txt
RUN pip3 install -r requirements.txt

COPY . .

ENV FLASK_ENV=development

EXPOSE ${PORT}
CMD [ "python3", "-m" , "flask", "run", "--host=0.0.0.0", "--port=4567"]

```
<img width="1216" alt="Screenshot 2023-02-20 at 4 55 39 PM" src="https://user-images.githubusercontent.com/1076924/221267851-a145389c-00f5-4608-b4d2-63cf35980e59.png">



Then build the container

`
docker build -t  backend-flask ./backend-flask
`

Run the container

```
docker run --rm -p 4567:4567 -it backend-flask
FRONTEND_URL="*" BACKEND_URL="*" docker run --rm -p 4567:4567 -it backend-flask
export FRONTEND_URL="*"
export BACKEND_URL="*"
docker run --rm -p 4567:4567 -it -e FRONTEND_URL='*' -e BACKEND_URL='*' backend-flask
docker run --rm -p 4567:4567 -it  -e FRONTEND_URL -e BACKEND_URL backend-flask
unset FRONTEND_URL="*"
unset BACKEND_URL="*"
```

Run the newly created image in the background and also return the container id to the environment variable. (Env Var)

`docker container run --rm -p 4567:4567 -d backend-flask`

`CONTAINER_ID=$(docker run --rm -p 4567:4567 -d backend-flask)`


## Contenerize the frontend

Since wwe are building the frontend for the app npm needs to be installed.

change directory into the frontend-react-js directory and install npm

`cd frontend-react-js`

`npm i`

3.  Create a dockerfile in the frontend-react-js directory (frontend-react-js/Dockerfile)

```
FROM node:16.18

ENV PORT=3000

COPY . /frontend-react-js
WORKDIR /frontend-react-js
RUN npm install
EXPOSE ${PORT}
CMD ["npm", "start"]
```

4.  Build container and run the container

`docker build -t frontend-react-js ./frontend-react-js`

`docker run -p 3000:3000 -d frontend-react-js`

## Running multiple containers

5.  Create a docker-compose.yml file in the root directory


```
version: "3.8"
services:
  backend-flask:
    environment:
      FRONTEND_URL: "https://3000-${GITPOD_WORKSPACE_ID}.${GITPOD_WORKSPACE_CLUSTER_HOST}"
      BACKEND_URL: "https://4567-${GITPOD_WORKSPACE_ID}.${GITPOD_WORKSPACE_CLUSTER_HOST}"
    build: ./backend-flask
    ports:
      - "4567:4567"
    volumes:
      - ./backend-flask:/backend-flask
  frontend-react-js:
    environment:
      REACT_APP_BACKEND_URL: "https://4567-${GITPOD_WORKSPACE_ID}.${GITPOD_WORKSPACE_CLUSTER_HOST}"
    build: ./frontend-react-js
    ports:
      - "3000:3000"
    volumes:
      - ./frontend-react-js:/frontend-react-js

# the name flag is a hack to change the default prepend folder
# name when outputting the image names
networks: 
  internal-network:
    driver: bridge
    name: cruddur
    
```
<img width="537" alt="Screenshot 2023-02-21 at 12 10 26 AM" src="https://user-images.githubusercontent.com/1076924/221271805-3fc561c8-f775-4169-b6fe-866c77038cee.png">


## Adding Postgres and Dynamodb into the existing dockerfile

For Postgres

```
services:
  db:
    image: postgres:13-alpine
    restart: always
    environment:
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=password
    ports:
      - '5432:5432'
    volumes: 
      - db:/var/lib/postgresql/data
volumes:
  db:
    driver: local
```


For DynamoDb Local

```
services:
  dynamodb-local:
    # https://stackoverflow.com/questions/67533058/persist-local-dynamodb-data-in-volumes-lack-permission-unable-to-open-databa
    # We needed to add user:root to get this working.
    user: root
    command: "-jar DynamoDBLocal.jar -sharedDb -dbPath ./data"
    image: "amazon/dynamodb-local:latest"
    container_name: dynamodb-local
    ports:
      - "8000:8000"
    volumes:
      - "./docker/dynamodb:/home/dynamodblocal/data"
    working_dir: /home/dynamodblocal
    
```
