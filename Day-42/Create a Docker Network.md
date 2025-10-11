# Day 42: Create a Docker Network

The **Nautilus DevOps** team has been assigned a task to set up several Docker networks for different applications. In this ticket, the objective is to create a Docker network named `ecommerce` on **App Server 2** in the **Stratos DC** environment. The task includes configuring the network with specific settings such as the network driver, subnet, and IP range.

## Task Breakdown

1. **Create a Docker Network named `ecommerce`**:
   - We need to create a Docker network called `ecommerce`.

2. **Configure the network to use the bridge driver**:
   - The bridge driver will allow containers to communicate with each other in the same host machine.

3. **Set the subnet to `172.28.0.0/24` and IP range to `172.28.0.0/24`**:
   - The network should use the specified subnet and IP range to manage IP addressing for the containers.

## Solution

To complete this task, we will run the following command on **App Server 2**:

```bash
docker network create \
  --driver bridge \
  --subnet 172.28.0.0/24 \
  --ip-range 172.28.0.0/24 \
  ecommerce
```
## Command Breakdown:
- docker network create: Creates a new Docker network.
- --driver bridge: Specifies the network driver as bridge (default for containers running on the same host).
- --subnet 172.28.0.0/24: Sets the subnet for the Docker network to 172.28.0.0/24.
- --ip-range 172.28.0.0/24: Specifies the IP range that Docker will use for containers in this network.
- ecommerce: The name of the Docker network being created.
## Verification
After running the command, we can verify the network creation:
List Docker networks:

```bash
docker network ls
```
Inspect the ecommerce network:
```bash
docker network inspect ecommerce
```
This will provide details about the network, including its driver, subnet, and IP range.

## Screenshots

<img width="700" height="500" alt="image" src="https://github.com/user-attachments/assets/a7b44116-fb60-4836-9983-8b728bc00256" />


