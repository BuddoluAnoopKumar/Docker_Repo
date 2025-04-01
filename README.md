# Docker_Repo
Sure! Let's walk through a detailed scenario in which we use various Docker commands to set up and run a simple web application. This example will involve a basic web server using Node.js, and we’ll cover commands commonly used in Docker:

### Scenario: Setting up a Node.js Web Application with Docker

**Overview:** We want to create a Docker container for a simple Node.js web application that responds with "Hello, World!" when accessed. We will cover building the image, running a container, and managing it. 

---

### Step-by-Step Execution

1. **Install Docker**  
   Ensure Docker is installed on your system. Follow the [official Docker installation guide](https://docs.docker.com/get-docker/).

2. **Create a Project Directory**  
   Open a terminal and create a new directory for your project:
   ```bash
   mkdir node-docker-app
   cd node-docker-app
   ```

3. **Create the Node.js Application**  
   Create a file named `app.js`:
   ```javascript
   // app.js
   const http = require('http');

   const hostname = '0.0.0.0'; // Use 0.0.0.0 to allow external access
   const port = 3000;

   const server = http.createServer((req, res) => {
       res.statusCode = 200;
       res.setHeader('Content-Type', 'text/plain');
       res.end('Hello World\n');
   });

   server.listen(port, hostname, () => {
       console.log(`Server running at http://${hostname}:${port}/`);
   });
   ```

4. **Create a `package.json` File**  
   Create a `package.json` to manage dependencies:
   ```json
   {
       "name": "node-docker-app",
       "version": "1.0.0",
       "description": "A simple Node.js app for Docker",
       "main": "app.js",
       "scripts": {
           "start": "node app.js"
       },
       "dependencies": {}
   }
   ```

5. **Create a Dockerfile**  
   Create a file named `Dockerfile` in the same directory:
   ```dockerfile
   # Use the official Node.js image as a base
   FROM node:14

   # Set the working directory inside the container
   WORKDIR /usr/src/app

   # Copy package.json and install dependencies
   COPY package.json ./
   RUN npm install

   # Copy the rest of the application code
   COPY . .

   # Expose the port that the app runs on
   EXPOSE 3000

   # Define the command to run the app
   CMD ["npm", "start"]
   ```

6. **Build the Docker Image**  
   Run the following command to build the Docker image:
   ```bash
   docker build -t node-docker-app .
   ```
   - **Explanation:** `docker build` creates a new image from the Dockerfile in the current directory (`.`), tagging it as `node-docker-app`.

7. **List Docker Images**  
   Verify the image was created successfully:
   ```bash
   docker images
   ```

8. **Run the Docker Container**  
   Now we need to run the container:
   ```bash
   docker run -d -p 3000:3000 --name my-node-app node-docker-app
   ```
   - **Explanation:** `docker run` runs the container in detached mode (`-d`), maps port 3000 of the container to port 3000 on the host (`-p`), and names the container `my-node-app`.

9. **Check Running Containers**  
   Verify that the container is running:
   ```bash
   docker ps
   ```

10. **Access the Application**  
    Open a browser and navigate to `http://localhost:3000`. You should see “Hello World”.

11. **View Container Logs**  
    To check the logs of your running container:
    ```bash
    docker logs my-node-app
    ```

12. **Stop the Container**  
    If you need to stop the application:
    ```bash
    docker stop my-node-app
    ```

13. **Restart the Container**  
    Restart the container if needed:
    ```bash
    docker start my-node-app
    ```

14. **Remove the Container**  
    If you want to remove the container:
    ```bash
    docker rm my-node-app
    ```

15. **Remove the Image**  
    After testing, if you wish to remove the image:
    ```bash
    docker rmi node-docker-app
    ```

### Summary of Docker Commands Used

- `docker build`: Builds a Docker image from a Dockerfile.
- `docker images`: Lists all images on your local Docker instance.
- `docker run`: Runs a command in a new container.
- `docker ps`: Lists running containers.
- `docker logs`: Fetches logs from a container.
- `docker stop`: Stops a running container.
- `docker start`: Starts a stopped container.
- `docker rm`: Removes a container.
- `docker rmi`: Removes an image.

### Conclusion
This scenario illustrates a complete workflow of creating, managing, and running a Dockerized Node.js application. You can adapt this flow to your needs, extending it to dependencies, databases, and more complex applications. Would you like to dive deeper into any specific aspect or command?
