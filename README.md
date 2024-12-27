# Devsecops
Deploy Netflix Clone on Cloud using Jenkins 
# DevSecOps Project

This repository contains the code and instructions for setting up a DevSecOps pipeline to automate the deployment of a Netflix-like application using AWS EC2, Docker, SonarQube, Jenkins, Prometheus, Grafana, and Kubernetes.

### Phase 1: Initial Setup and Deployment

#### Step 1: Launch EC2 (Ubuntu 22.04)

1. Provision an EC2 instance on AWS with Ubuntu 22.04.
2. Connect to the instance using SSH.

#### Step 2: Clone the Code

1. Update all packages:
    ```bash
    sudo apt-get update
    ```

2. Clone the repository:
    ```bash
    git clone https://github.com/N4si/DevSecOps-Project.git
    ```

#### Step 3: Install Docker and Run the App Using a Container

1. Install Docker:
    ```bash
    sudo apt-get install docker.io -y
    sudo usermod -aG docker $USER
    newgrp docker
    sudo chmod 777 /var/run/docker.sock
    ```

2. Build and run the Docker container:
    ```bash
    docker build -t netflix .
    docker run -d --name netflix -p 8081:80 netflix:latest
    ```

3. To delete a container:
    ```bash
    docker stop <containerid>
    docker rmi -f netflix
    ```

#### Step 4: Get the API Key

1. Go to TMDB (The Movie Database) website and create an account.
2. Navigate to your profile settings, and create a new API key.
3. Rebuild the Docker image with your API key:
    ```bash
    docker build --build-arg TMDB_V3_API_KEY=<your-api-key> -t netflix .
    ```

---

### Phase 2: Security

#### Install SonarQube and Trivy

1. Install SonarQube:
    ```bash
    docker run -d --name sonar -p 9000:9000 sonarqube:lts-community
    ```

    Access SonarQube at: `publicIP:9000` (Default credentials: `admin/admin`).

2. Install Trivy:
    ```bash
    sudo apt-get install wget apt-transport-https gnupg lsb-release
    wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | sudo apt-key add -

