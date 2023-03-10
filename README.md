Add the below content in our docker-compose.yml file:

version: '3.4'
services:
  super-app-db:
    image: mysql:8.0.28
    environment:
      MYSQL_DATABASE: 'super-app'
      MYSQL_ROOT_PASSWORD: '$SuperApp1'
    ports:
      - '3306:3306'
    expose:
      - '3306'
      Under the services section we will list all the types of applications to be configured.

To start with we configure a super-app-db service which pulls a Docker image of MySQL with version 8.0.28.

Next, we instruct the container to create a database name super-app
Lastly, since the default port for MySQL is 3306, we are mapping it to the container’s port 3306 and exposing that port for access.
Once the above file is created, run the "docker compose up" command to get our Docker image created and run it as a container.

We will create a very simple Express Node application. In order to do so, create a folder named node inside our super-app folder.

Add the files server.js, package.json and Dockerfile in the node folder.

server.js:

const server = require("express")();
server.listen(3000, async () => { });
server.get("/super-app", async (_, response) => {
    response.json({ "super": "app" });
});
package.json:

{
    "name": "super-app-node",
    "dependencies": {
        "express": "^4.17.1"
    }
}

Here we are creating a Node application that returns JSON when we hit localhost:3000/super-app in the browser. Now, we are not directly going to run the project from this folder.

Instead, go back to our super-app folder and append the below lines to our docker-compose.yml file:

  super-app-node:
    build: ./node
    ports:
      - "3000:3000"
      We are simply mentioning to create a service named super-app-node. We are also mapping the container port to the host port 3000.

Finally run the "docker compose up" to run our two containers (MySQL and NodeJS)
