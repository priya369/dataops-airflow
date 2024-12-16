# Run Apache Airflow Locally Using Docker

This guide provides instructions to set up and run an Apache Airflow server on your local machine using Docker.

## Prerequisites

1. **Install Docker Desktop**  
   - Follow the [Docker Desktop installation guide](https://docs.docker.com/docker-for-windows/install/).
   - **For Windows Users**: Enable Windows Subsystem for Linux (WSL) before installing Docker.

---

## What is Docker?

Docker simplifies creating, deploying, and running applications by using containers. Containers package applications with all their dependencies, ensuring they run consistently across any environment.

---

## Steps to Set Up Apache Airflow

### Step 1: Prepare Local Directory

1. Create a directory on your machine.
2. Place the `docker-compose.yaml` file inside this directory.

---

### Step 2: Build and Start Airflow

#### First-Time Setup

1. Build the required Docker images by running the following commands:
   ```bash
   SET VOLUME_HOST_PATH=[Path to directory on host]
   docker-compose up airflow-init

.Replace [Path to directory on host] with the directory path where you want to store volume data.
  ```bash
  SET VOLUME_HOST_PATH=/c/tmp

.It is recommended to place the volume in the same directory as docker-compose.yaml for better organization.

# Instructions to Start Existing Airflow Service

If you have already created the service and you would like to get it up and running, navigate to the directory where you have your `docker-compose.yaml` file located and run the following command:

```bash
docker-compose up
# Instructions after starting Airflow Service

Once the service is up, you will see there are a handful of containers running in the
background, which are interacting with each other to standup your Airflow server
```bash
docker-compose ps

# Instructions to access airflow on browser

Open your web-browser and navigate to localhost:8080 (also know as
127.0.0.1:8080), which should bring you to the login page. The default username and
password are set to “airflow” and “airflow”. You can see this on lines 118 and 119 in the
docker-compose.yaml file, under the airflow-init block’s environment variables.
A successfully login with bring you the Airflow UI, shown in the subsequent image.

