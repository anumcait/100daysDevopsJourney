# Day 44 - Host Static Website with Docker Compose

This project sets up a simple static website using the `httpd` web server (Apache) in a Docker container, managed with Docker Compose.

## 🚀 Objective

Deploy a static website using Docker Compose by running an `httpd` container that serves content from a specified host directory.


## 🐳 Docker Compose Configuration

- **Image**: `httpd:latest`
- **Container Name**: `httpd`
- **Port Mapping**: Host `8087` → Container `80`
- **Volume Mapping**: Host `/opt/finance` → Container `/usr/local/apache2/htdocs`

## 📦 docker-compose.yml

```yaml
version: '3'

services:
  webserver:
    image: httpd:latest
    container_name: httpd
    ports:
      - "8087:80"
    volumes:
      - /opt/finance:/usr/local/apache2/htdocs
```
🛠️ How to Use

### Ensure Docker is installed and docker compose command is available:
```bash
docker compose version
```
### Navigate to the Docker project directory:
```
cd /opt/docker
```
### Start the container:
```
docker compose up -d
```

### Verify the container is running:
```
docker ps
```
### Test the website:
```
curl http://localhost:8087
```
You should see the HTML content served from /opt/finance.

### 🧹 To Stop and Remove the Container
```
docker compose down
```
### Screenshots:

<img width="700" height="500" alt="image" src="https://github.com/user-attachments/assets/7e814ac1-4331-445d-97a9-dccbbe5f9740" />

