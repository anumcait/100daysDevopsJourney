## Day 47: Docker Python App

A python app needed to be Dockerized, and then it needs to be deployed on App Server 1. 

We have already copied a requirements.txt file (having the app dependencies) under /python_app/src/ directory on App Server 1. 

Further complete this task as per details mentioned below: Create a Dockerfile under /python_app directory: Use any python image as the base image. 

Install the dependencies using requirements.txt file. Expose the port 8082. Run the server.py script using CMD. 

Build an image named nautilus/python-app using this Dockerfile. Once image is built, create a container named pythonapp_nautilus: Map port 8082 of the container to the host port 8098. 

Once deployed, you can test the app using curl command on App Server 1.

---

## Steps to Dockerize and Deploy the Python App

### 1. **Create the Dockerfile**

Create a `Dockerfile` in the `/python_app` directory with the following content:

```dockerfile
# Step 1: Use an official Python runtime as the base image
FROM python:3.9-slim

# Step 2: Set the working directory inside the container
WORKDIR /python_app

# Step 3: Copy the requirements.txt to the container
COPY src/requirements.txt .

# Step 4: Install the dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Step 5: Copy the rest of the application code into the container
COPY src/ .

# Step 6: Expose port 8082
EXPOSE 8082

# Step 7: Set the default command to run the server
CMD ["python", "server.py"]

```
## 2. Build the Docker Image

Once the Dockerfile is ready, you can build the Docker image. Run the following command on App Server 1:
```bash
docker build -t nautilus/python-app /python_app
```

This will:
- Build the image and tag it as nautilus/python-app.
- Use the /python_app directory where the Dockerfile and requirements.txt are located.

## 3. Create and Run the Container
Now, create and run the container using the following command:
```bash
docker run -d --name pythonapp_nautilus -p 8098:8082 nautilus/python-app
```

Explanation of the flags:

- -d: Run the container in detached mode (in the background).
- --name pythonapp_nautilus: Name the container pythonapp_nautilus.
- -p 8098:8082: Map port 8082 inside the container to port 8098 on the host machine.
- nautilus/python-app: The Docker image to use for creating the container.

## 4. Test the Python App

Once the container is running, you can test if the app is successfully deployed by using curl:
```bash
curl http://localhost:8098
```

This should send a request to the Python app running inside the container. If everything is set up correctly, you should get a response from the app.

## Screenshots

<img width="700" height="500" alt="Screenshot 2025-10-16 205018" src="https://github.com/user-attachments/assets/15e2fec8-1619-4b5c-968a-0b49664a0572" />

