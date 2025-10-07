# Day 38: Docker Image Task - Nautilus Project

## Objective

As part of the Nautilus project, the developers are planning to test containerized environment application features. 

The task involves pulling the `busybox:musl` Docker image on App Server 3 in Stratos Data Center (DC) and re-tagging it with a new tag `busybox:news`.

## Steps to Accomplish the Task

### Step 1: Log into App Server 3 in Stratos DC

Ensure that you have access to App Server 3 and that Docker is installed and running. If you are not already logged in, connect to the server using SSH:

```bash
ssh banner@stapp03
```
### Step 2: Pull the busybox:musl Image

Once you're logged in, pull the busybox:musl image from Docker Hub using the following command:
```bash
docker pull busybox:musl
```

This will download the busybox:musl image to the local Docker registry on the App Server 3.

### Step 3: Re-tag the Image as busybox:news

After the image is pulled successfully, you need to create a new tag (busybox:news) for the busybox:musl image. Run the following command:
```bash
docker tag busybox:musl busybox:news
```

This command tags the already downloaded image with the new tag busybox:news.

### Step 4: Verify the Tagging

To verify that the image has been tagged correctly, list all available Docker images using:
```bash
docker images
```
You should see an entry for busybox with both the tags busybox:musl and busybox:news.

### Summary of Commands

- Log into App Server 3:
    - ssh user@app-server-3.stratos-dc
- Pull busybox:musl Image:
    - docker pull busybox:musl
- Re-tag the Image:
    - docker tag busybox:musl busybox:news
- Verify the Tagging:
    - docker images
 
### Screenshots
<img width="700" height="500" alt="image" src="https://github.com/user-attachments/assets/56262c45-dc44-444c-9fc2-3355cccce640" />
<img width="700" height="500" alt="image" src="https://github.com/user-attachments/assets/d272ba97-751d-4065-b446-bfcb789d1117" />


