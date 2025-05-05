# Simple Flask Server with CI/CD Pipeline

This repository contains a basic Flask server with a CI/CD pipeline using Jenkins and Docker. The server is automatically deployed in a Docker container and cleaned up after 7 days.

## Project Structure

```
├── app.py                 # Main Flask server
├── requirements.txt       # Python dependencies
├── Dockerfile            # Docker configuration
├── Jenkinsfile          # Jenkins pipeline definition
└── README.md            # Project documentation
```

## Features

- Simple Flask server returning "Hello World"
- Automated Docker build and deployment using Jenkins
- Automatic container cleanup after 7 days
- Windows-compatible Jenkins pipeline

## Prerequisites

1. Jenkins installed on Windows
2. Docker installed and running
3. Python 3.x installed

## Setup Instructions

### 1. Setting up the Flask Server

1. Clone this repository:
   ```bash
   git clone https://github.com/your-username/flask-server-cicd.git
   cd flask-server-cicd
   ```

2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

3. Run the server locally (optional):
   ```bash
   python app.py
   ```
   The server will be available at http://localhost:5000

### 2. Setting up Jenkins Pipeline

1. Install Jenkins:
   - Download the Windows installer (.msi) from https://www.jenkins.io/download/
   - Run the installer and follow the setup wizard
   - Jenkins will be available at http://localhost:8080

2. Install required Jenkins plugins:
   - Go to "Manage Jenkins" > "Manage Plugins" > "Available"
   - Install:
     - Git plugin
     - Pipeline plugin
     - Docker plugin

3. Create a new Pipeline:
   - Click "New Item"
   - Enter a name (e.g., "flask-server-cicd")
   - Select "Pipeline"
   - Click "OK"
   - In the configuration:
     - Under "Pipeline", select "Pipeline script from SCM"
     - Select "Git" as SCM
     - Enter your GitHub repository URL
     - Set branch to */main
     - Script Path: Jenkinsfile
   - Click "Save"

## How it Works

1. When code is pushed to the GitHub repository:
   - Jenkins pulls the latest code
   - Builds a Docker image
   - Stops and removes any existing container
   - Deploys a new container
   - Creates a scheduled task to remove the container after 7 days

2. The Docker container:
   - Runs on port 5000
   - Is automatically removed after 7 days
   - Can be accessed at http://your-server:5000

## Environment Variables

The following variables are defined in the Jenkinsfile:
- DOCKER_IMAGE: flask-server-cicd
- CONTAINER_NAME: flask-server-cicd
- CONTAINER_PORT: 5000

## Notes

- The container cleanup is handled by Windows Task Scheduler
- If deployment fails, the pipeline will automatically clean up any existing containers
- The server runs in debug mode for demonstration purposes 