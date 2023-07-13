First I forked the repository from the IP to my repository on github. then I clone it to my machine using 'git clone'. THen opened the app using my vscode to build and run the app using the following steps as explained below:-

Choice of Image I have used the node:lts-alpine image for the backend and frontend services. This is because its small size and the long term support which makes it a stable version to choose and it is easy to pull. For the db service, I have used 'mongo:4.0'.

Dockerfile. (Dockerfile.backend.dev & Dockerfile.client.dev) The backend and frontend services have these directives 
    a) FROM - this is the base image and I am using node:lts-alpine. 
    b) WORKDIR - this is the directory our application will be running and I have used /usr/app. I have used the /usr dir to avoid any conflicting directory with /app 
    c) COPY -> ./package*.json ./ - This will be used to copy the package.json file from the machine to our container. We copy package.json first to our container then install the packages to avoid a situation where our container takes a lot of time to build.

    d) RUN npm install - This command will install all the packages on our package.json file

    e) COPY . . - After copying package.json, we will copy the remaining contents to our container

    f) Exposing the port which will be used to access the client from the local machine through port 3000 and the backend service and port 5000 for the backend service.

    f) CMD ["npm", "run", "dev"] - This command runs our application on dev environment. This is for backend.

    g) CMD ["npm", "start"] - This is for the client container and we are starting the client dev server with this command.

Docker Compose File. I created a docker compose file named 'docker-compose.yml' on the root directory of the project. This is becasue, we need to create services for backend, client and database. This is a shortcut way or easier way of running all your containers at once and starting the applicationand DB with one command 'docker compose up'

Each folder in the root directory,  I have created its dockerfile separate and defined what needto be run in the service. so the docker compose file has a service for backend for the backend service, frontend for the react app and mongodb for the database. inside there also I have defined the volumes to attach to the containers for saving data.

Networks - I have created a user defined network called 'yolonetwork'. All containers are tagged to use this network for communication.

Publish to Docker Hub To publish this images to your docker hub, first you need to log into your docker hub account. Run 'docker login '. Provide your docker hub username and password. Push the images to your docker up with these commands:

docker build -t dockerhubusername/containername:version -f ./dir for dockerfile (a) backend: docker build -t muchirisamuel/backend:v1.0.0 -f ./backend/Dockerfile.backend.dev ./backend (b) client: docker build -t muchirisamuel/client:v1.0.0 -f ./client/Dockerfile.client.dev ./client

docker push to docker hub. (a) backend: docker push muchirisamuel/yolo-backend:v1.0.0
(b) client: docker push muchirisamuel/yolo-client:v1.0.0
