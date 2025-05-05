# Simple Flask Server with CI/CD Pipeline

This repository contains a basic Flask server with a CI/CD pipeline using Jenkins and Codeberg.

## Project Structure

```
├── app.py                 # Main Flask server
├── requirements.txt       # Python dependencies
├── Jenkinsfile            # Jenkins pipeline definition
└── README.md              # Project documentation
```

## Setup Instructions

### 1. Setting up the Flask Server

1. Clone this repository:
   ```bash
   git clone https://codeberg.org/your-username/flask-server-cicd.git
   cd flask-server-cicd
   ```

2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

3. Run the server locally:
   ```bash
   python app.py
   ```
   The server will be available at http://localhost:5000

### 2. Setting up Jenkins

1. Install Jenkins:
   - Download the Windows installer (.msi) from https://www.jenkins.io/download/
   - Run the installer and follow the setup wizard
   - Jenkins will be available at http://localhost:8080

2. Access Jenkins:
   - Open http://localhost:8080 in your browser
   - Follow the initial setup wizard
   - Install suggested plugins
   - Create an admin user

3. Configure Jenkins:
   - Install additional plugins: Go to "Manage Jenkins" > "Manage Plugins" > "Available"
     - Git plugin
     - Pipeline plugin
     - Generic Webhook Trigger plugin

4. Create a new Pipeline:
   - Click "New Item"
   - Enter a name (e.g., "flask-server-cicd")
   - Select "Pipeline"
   - Click "OK"
   - In the configuration page:
     - Under "Pipeline", select "Pipeline script from SCM"
     - Select "Git" as the SCM
     - Enter your Codeberg repository URL
     - Set the branch to */main
     - Script Path: Jenkinsfile
     - Click "Save"

### 3. Setting up the Webhook

1. Configure the Generic Webhook Trigger in Jenkins:
   - Go to your pipeline job
   - Click "Configure"
   - Check "Generic Webhook Trigger" under "Build Triggers"
   - Set a token (e.g., "flask-webhook-token")
   - Click "Save"

2. For local development, set up a tunnel service to expose your Jenkins:
   - Install ngrok: https://ngrok.com/download
   - Run ngrok to create a tunnel to your Jenkins:
     ```bash
     # If Jenkins is running on port 8080
     ngrok http 8080
     ```
   - Note the URL provided by ngrok (e.g., https://abcd1234.ngrok.io)

3. Configure the webhook in Codeberg:
   - Go to your repository on Codeberg
   - Click "Settings" > "Webhooks" > "Add Webhook"
   - Set the Payload URL to: `https://your-ngrok-url/generic-webhook-trigger/invoke?token=YOUR_TOKEN`
   - Content type: application/json
   - Select "Just the push event"
   - Click "Add Webhook"

## How the Automation Works

1. **Trigger**: When code is pushed to the Codeberg repository, the webhook sends a notification to Jenkins.

2. **Pipeline Execution**:
   - Jenkins receives the webhook and triggers the pipeline
   - The pipeline checks out the latest code
   - It installs the necessary dependencies
   - It deploys the Flask server

3. **Deployment**: The server is deployed on Windows by:
   - Killing any existing process running on port 5000
   - Starting the Flask app with Python in the background

## Assumptions and Limitations

- The server running Jenkins needs to have Python installed
- The port 5000 needs to be available for the Flask server
- For local development with webhooks, a tunnel service like ngrok is required
- This setup is optimized for Windows environments 