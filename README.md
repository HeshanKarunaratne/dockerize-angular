# Containerzie Angular App

### Steps
#### Docker
1. Use `ng new <project-name>` to generate an Angular application
2. Use `npm start` to serve the application in port 4200
3. Use below Dockerfile

~~~Dockerfile
FROM node:alpine
WORKDIR '/app'
COPY package.json .
RUN npm install
COPY . .
EXPOSE 4200
CMD npm run start
~~~

4. Use `docker build -t <tag-name> .` to build and `docker images` to check the images created
5. Use `docker run -p 4200:4200 <image-id>` to run a container
6. check `docker ps` and see the port mapping
7. Create a `docker-compose.yml` file to configure Dockerfile

~~~yml
version: "3"
services:
  angular-app:
    build: .
    ports:
      - "4200:4200"
    volumes:
      - .:/app
~~~

8. Add volume mappings for `node_modules` and `all the copied files`
9. Make sure to add below to `package.json`

~~~txt
"start": "ng serve --host 0.0.0.0"
~~~

#### Elastic Beanstalk
10. Navigate to Elastic Beanstalk and create a new application
11. Will be using `eb cli` to setup the application
12. Install using `pip install awsebcli --upgrade --user` 
13. Add below path to environement 

~~~text
%USERPROFILE%\AppData\Roaming\Python\Python311\Scripts
~~~

14. Check version using `eb --version`
15. We will be using multi step docker file

~~~Dockerfile
FROM node:alpine AS BUILD
WORKDIR '/app'
COPY package.json .
RUN npm install
COPY . .
RUN npm run build

FROM nginx
EXPOSE 80
COPY --from=BUILD /app/dist/my-angular-app /usr/share/nginx/html
~~~

16. Execute `docker run -p 80:80 <image-id>` and navigate to browser and run `http://localhost/80` and it will serve the application through `nginx`
17. Make sure your AWS profile is configured
18. Type `eb init` to start the creation of application