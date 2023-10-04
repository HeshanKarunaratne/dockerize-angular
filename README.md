# Containerzie Angular App

### Steps
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