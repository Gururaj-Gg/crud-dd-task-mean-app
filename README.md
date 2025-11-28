# FULL STACK MEAN Docker Deployment 

**Gururaj G**

## 🌐 Live Application
🔗 http://47.129.194.69/

## 📌 Project Overview
This project demonstrates how to **set up, containerize, and deploy a full-stack MEAN (MongoDB, Express, Angular, Node.js) application** using **Docker, Docker Compose, CI/CD with GitHub Actions, Nginx Reverse Proxy, and AWS EC2**. 
It includes automated deployment where every push to the `main` branch triggers the CI/CD pipeline to rebuild Docker images, push them to Docker Hub, pull on the server, and restart services.

---

## 🏗 Technology Stack
| Layer | Technology |
|--------|------------|
| Frontend | Angular |
| Backend | Node.js + Express |
| Database | MongoDB |
| Reverse Proxy | Nginx |
| Containerization | Docker, Docker Compose |
| CI/CD | GitHub Actions |
| Cloud Hosting | AWS EC2 (Ubuntu) |

---

## Project setup

### Node.js Server

cd backend

npm install

You can update the MongoDB credentials by modifying the `db.config.js` file located in `app/config/`.

Run `node server.js`
npm install -g @angular/cli

### Angular Client

cd frontend

npm install

Run `ng serve --port 8081`

You can modify the `src/app/services/tutorial.service.ts` file to adjust how the frontend interacts with the backend.

Navigate to `http://localhost:8081/`


## 📂 Project Structure

crud-dd-task-mean-app/
├── frontend/
│ └── Dockerfile

├── backend/
│ └── Dockerfile

├── docker-compose.yml
└── README.md

## 🖥 Architecture Diagram    

                         ┌────────────────────┐
                         │  GitHub Actions     │
                         │     (CI/CD)         │
                         └──────────┬──────────┘
                                    │
                                    ▼
                         ┌────────────────────┐
                         │  Docker Hub         │
                         │  Container Registry │
                         └──────────┬──────────┘
                                    │
                                    ▼
                         ┌────────────────────┐
                         │ AWS EC2 Ubuntu VM   │
                         │   (Production)      │
                         └──────────┬──────────┘
                                    │
                                    ▼
                         ┌────────────────────┐
                         │  Docker Compose     │
                         └──────────┬──────────┘
                                    │
            ┌──────────────┬────────┴─────────┬──────────────┐
            │              │                  │              │
            ▼              ▼                  ▼              ▼
       ┌────────┐     ┌──────────┐      ┌──────────┐     ┌──────────┐
       │ Browser│ --->│  Nginx    │ --->│ Frontend  │     │ Backend   │
       │ Client │     │ Reverse   │     │ Angular   │     │ API       │
       └────────┘     │  Proxy    │     └──────────┘     └─────┬──────┘
                       └──────────┘                               │
                                                                  ▼
                                                          ┌─────────────┐
                                                          │  MongoDB     │
                                                          │  Database    │
                                                          └─────────────┘



## Screenshots

## Application UI running	http://47.129.194.69/ and Nginx

<img width="1860" height="989" alt="Screenshot 2025-11-28 141455" src="https://github.com/user-attachments/assets/679667ff-8d8a-457f-8b44-9ead4abae94d" />

## Docker images in Docker Hub , mean-frontend / mean-backend

<img width="1920" height="1080" alt="Screenshot 2025-11-28 152303" src="https://github.com/user-attachments/assets/7c0ea729-8243-49e3-af0a-af9056a29118" />

## GitHub Actions Successful Pipeline Actions page

<img width="1849" height="1012" alt="Screenshot 2025-11-28 145643" src="https://github.com/user-attachments/assets/ffee384a-2bd8-4843-8f14-e0133b308e9a" />

## AWS EC2 Instance dashboard Instance details

<img width="1905" height="675" alt="Screenshot 2025-11-28 152829" src="https://github.com/user-attachments/assets/8cc9a5c2-53d6-45fb-b5b5-1bb29008ea7d" />

# project folder view Dockerfile + Compose presence

<img width="1920" height="1080" alt="Screenshot (35)" src="https://github.com/user-attachments/assets/32b8fb28-8edd-46c6-80f8-30fc75380476" />

## Nginx Infrastructure and setup

                           User / Browser
                                   │
                          http://47.129.194.69
                                   │
                           ┌───────────────┐
                           │    NGINX       │   ← Reverse Proxy
                           └───────────────┘
                         /                     \
                        /                       \
            / (Frontend requests)          /api/ (API requests)
                │                                │
     Angular Frontend Container        Node.js Backend Container
     mean-frontend:80                   mean-backend:3000
                                                │
                                        MongoDB Container


## Nginx Configuration File (nginx.conf)

server {
    listen 80;

    # Serve Angular Frontend at root path /
    location / {
        proxy_pass http://mean-frontend:80;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }

    # Proxy backend API requests
    location /api/ {
        proxy_pass http://mean-backend:3000/;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}

## Nginx Docker Compose Setup

nginx:
  
  image: nginx:alpine
  
  container_name: mean-nginx
  
  restart: always
 
  ports:
    - "80:80"
  
  volumes:
    - ./nginx/nginx.conf:/etc/nginx/conf.d/default.conf:ro
  
  depends_on:
    - frontend
    - backend


## 🔧 CI/CD Configuration Steps

Step 1 — Create Docker Hub Access Token

Login to Docker Hub → Account Settings

Go to Security → Access Tokens

Click New Access Token

Name: github-actions

Permission: Read/Write

Copy the generated token

Step 2 — Add GitHub Secrets

GitHub → Repository → Settings → Secrets → Actions → New repository secret

| Secret Name          | Example Value           |
| -------------------- | ----------------------- |
| `DOCKERHUB_USERNAME` | gururajg9               |
| `DOCKERHUB_TOKEN`    | Docker Hub Access Token |
| `VM_SSH_HOST`        | 47.129.194.69           |
| `VM_SSH_USER`        | ubuntu                  |
| `VM_SSH_PRIVATE_KEY` | Contents of .pem key    |
| `VM_SSH_PORT`        | 22                      |


Step 3 — Create GitHub Actions Workflow

Create file:.github/workflows/deploy.yml

name: CI/CD Deploy to EC2

on:
  push:
    branches: ["main"]

jobs:
  build-and-push:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repo
        uses: actions/checkout@v4

      - name: Login to Docker Hub
        uses: docker/login-action@v3
        with:
          username: ${{ secrets.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}

      - name: Build and push backend
        uses: docker/build-push-action@v6
        with:
          context: ./backend
          push: true
          tags: ${{ secrets.DOCKERHUB_USERNAME }}/mean-backend:latest

      - name: Build and push frontend
        uses: docker/build-push-action@v6
        with:
          context: ./frontend
          push: true
          tags: ${{ secrets.DOCKERHUB_USERNAME }}/mean-frontend:latest

  deploy:
  
    needs: build-and-push
    runs-on: ubuntu-latest
    
    steps:
      - name: Deploy on EC2
        uses: appleboy/ssh-action@v0.1.10
        with:
          
          host: ${{ secrets.VM_SSH_HOST }}
          
          username: ${{ secrets.VM_SSH_USER }}
         
          key: ${{ secrets.VM_SSH_PRIVATE_KEY }}
          
          port: ${{ secrets.VM_SSH_PORT }}
         
          script: |
           
  cd ~/crud-dd-task-mean-app
         
            git pull
            
            docker compose pull
            
            docker compose up -d
            
            docker image prune -f
            
## 1. Developer pushes code to GitHub
git add .

git commit -m "update"

git push origin main

## 2. GitHub Actions is triggered

<img width="1849" height="1012" alt="Screenshot 2025-11-28 145643" src="https://github.com/user-attachments/assets/7ecb7822-7da1-48eb-b581-c762c01cf846" />

## Docker image build and push process

cd crud-dd-task-mean-app

docker build -t gururajg9/mean-backend:latest ./backend

docker push gururajg9/mean-backend:latest

docker build -t gururajg9/mean-frontend:latest ./frontend

docker push gururajg9/mean-frontend:latest

## 🧾 Result After pushing, open Docker Hub and check

<img width="1590" height="900" alt="Screenshot 2025-11-28 102237" src="https://github.com/user-attachments/assets/bf018ae1-fce6-4a54-9f4d-308ccdae3798" />


