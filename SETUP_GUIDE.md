# Step-by-Step Setup Guide

This guide will walk you through the complete setup of the CI/CD pipeline for the Flask server using Codeberg and Jenkins.

## 1. Setting Up the Codeberg Repository

1. Create an account on Codeberg if you don't have one already: https://codeberg.org/user/sign_up

2. Create a new repository:
   - Click on the "+" icon at the top right corner
   - Select "New Repository"
   - Name it "flask-server-cicd" (or any name you prefer)
   - Add a description if desired
   - Initialize with a README file
   - Click "Create Repository"

3. Clone the repository to your local machine:
   ```bash
   git clone https://codeberg.org/your-username/flask-server-cicd.git
   cd flask-server-cicd
   ```

4. Add all the project files to your repository:
   - app.py
   - requirements.txt
   - Jenkinsfile
   - README.md
   - .gitignore

5. Commit and push the files:
   ```bash
   git add .
   git commit -m "Initial commit"
   git push origin main
   ```

## 2. Setting Up Jenkins

### Installing Jenkins

#### For Windows:

1. Download the Windows installer (.msi) from https://www.jenkins.io/download/
2. Run the installer and follow the setup wizard
3. Jenkins will be installed as a Windows service and will start automatically
4. Open a browser and go to http://localhost:8080

### Initial Jenkins Setup

1. Retrieve the initial admin password:
   - For Windows: Open `C:\Program Files (x86)\Jenkins\secrets\initialAdminPassword`

2. Open http://localhost:8080 in your browser
3. Enter the initial admin password
4. Choose "Install suggested plugins"
5. Create an admin user when prompted
6. Complete the setup wizard

### Install Required Plugins

1. Go to "Manage Jenkins" > "Manage Plugins" > "Available"
2. Search and install the following plugins:
   - Git Integration (should be pre-installed)
   - Pipeline (should be pre-installed)
   - Generic Webhook Trigger
3. Click "Install without restart"
4. Check "Restart Jenkins when installation is complete and no jobs are running"

## 3. Creating a Jenkins Pipeline

1. Click "New Item" on the Jenkins dashboard
2. Enter a name for your pipeline (e.g., "flask-server-cicd")
3. Select "Pipeline" and click "OK"
4. In the configuration page:
   - Under "General", check "GitHub project" and enter your Codeberg repository URL
   - Under "Build Triggers", check "Generic Webhook Trigger"
   - Add a variable:
     - Variable: payload
     - Expression Type: JSONPath
     - Expression: $.payload
   - Set a token (e.g., "flask-webhook-token")
   - Under "Pipeline", select "Pipeline script from SCM"
   - SCM: Git
   - Repository URL: Your Codeberg repository URL
   - Credentials: Add your Codeberg credentials if the repository is private
   - Branch Specifier: */main
   - Script Path: Jenkinsfile
5. Click "Save"

## 4. Setting Up a Tunnel for Local Development

For webhook triggers to work with local Jenkins, you need to expose your Jenkins server to the internet. You can use ngrok for this:

1. Download ngrok from https://ngrok.com/download
2. Start ngrok to create a tunnel to your Jenkins:
   ```bash
   # If Jenkins is running on port 8080
   ngrok http 8080
   ```
3. Note the URL provided by ngrok (e.g., https://abcd1234.ngrok.io)

## 5. Configuring the Webhook in Codeberg

1. Go to your repository on Codeberg
2. Click "Settings" > "Webhooks" > "Add Webhook"
3. Configure the webhook:
   - Payload URL: `https://your-ngrok-url/generic-webhook-trigger/invoke?token=YOUR_TOKEN`
   - Content type: application/json
   - Secret: Leave blank (or set if desired)
   - Trigger On: Just the push event
4. Click "Add Webhook"

## 6. Testing the CI/CD Pipeline

1. Make a change to your local repository:
   ```bash
   # For example, update the message in app.py
   git add .
   git commit -m "Update server message"
   git push origin main
   ```

2. Go to your Jenkins dashboard and check if the pipeline was triggered
3. Check the console output to see if the pipeline stages executed successfully
4. Access your Flask server (http://your-server-ip:5000) to verify the deployment

## 7. Troubleshooting

### Webhook not triggering:
- Check that ngrok is running and the URL is correct
- Verify the webhook configuration in Codeberg
- Check Jenkins logs for any webhook-related issues

### Pipeline failing:
- Check the console output for error messages
- Verify that the Jenkinsfile is correct and compatible with your environment
- Ensure all required tools (Python, pip) are installed on the Jenkins server

### Server not starting:
- Check if port 5000 is already in use
- Look for error messages in the console output
- Try running the server manually to see if there are any issues 