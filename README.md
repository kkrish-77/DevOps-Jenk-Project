# Simple Flask Server with CI/CD Pipeline

# Jenkins CI/CD Pipeline for Flask App 🚀

This project demonstrates a simple CI/CD pipeline using **Jenkins** to:

- Clone a GitHub repo
- Build a Docker image for a Flask app
- (Optionally) Run the Docker container

## 📁 Project Files

- `app.py` – Flask application
- `Dockerfile` – Instructions to build the Docker image
- `requirements.txt` – Python dependencies
- `Jenkinsfile` – Jenkins pipeline script

## ⚙️ Jenkins Pipeline Steps

1. **Clone Repository**  
2. **Build Docker Image**  
3. **Run Container** (optional)

## 🐳 Quick Docker Run (Optional)

```bash
docker build -t flask-app .
docker run -d -p 5000:5000 flask-app


- The container cleanup is handled by Windows Task Scheduler
- If deployment fails, the pipeline will automatically clean up any existing containers
- The server runs in debug mode for demonstration purposes 
