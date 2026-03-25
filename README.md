# Jenkins CI/CD Pipeline Project

Automated pipeline: GitHub push triggers Jenkins → builds Docker image
→ pushes to DockerHub → deploys container on EC2.

## Pipeline Stages
| Stage | What it does |
|-------|-------------|
| Checkout | Pulls latest code from GitHub |
| Build | Builds Docker image |
| Push | Pushes to DockerHub |
| Deploy | Runs container on port 5000 |

## Access
- Jenkins UI: http://EC2_IP:8080
- Flask App:  http://EC2_IP:5000

## Tech Stack
Jenkins | Docker | DockerHub | GitHub Webhooks | Python Flask | AWS EC2 server |
