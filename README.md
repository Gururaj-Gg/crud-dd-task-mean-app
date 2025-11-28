# MEAN Stack Docker Deployment Project

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
├── frontend/ # Angular UI
│ └── Dockerfile
├── backend/ # Node/Express API
│ └── Dockerfile
├── docker-compose.yml # Multi-container deployment setup
└── README.md

## 🖥 Architecture Diagram    

               GitHub Actions (CI/CD)
                       │
                       ▼
              Docker Hub Registry
                       │
                       ▼
               AWS EC2 Ubuntu Server
        ┌──────────────┴────────────────┐
        │        Docker Compose          │
        │  ┌────────┐   ┌──────────┐    │
Browser ─► │ Nginx  │──►│ Frontend │    │
        │  └────────┘   └──────────┘    │
        │         │           │          │
        │         ▼           ▼          │
        │     Backend API  MongoDB       │
        └────────────────────────────────┘

## Screenshots

## Application UI running	http://47.129.194.69/

<img width="1860" height="989" alt="Screenshot 2025-11-28 141455" src="https://github.com/user-attachments/assets/679667ff-8d8a-457f-8b44-9ead4abae94d" />

## Docker images in Docker Hub , mean-frontend / mean-backend

<img width="1920" height="1080" alt="Screenshot 2025-11-28 152303" src="https://github.com/user-attachments/assets/7c0ea729-8243-49e3-af0a-af9056a29118" />

## GitHub Actions Successful Pipeline Actions page

<img width="1849" height="1012" alt="Screenshot 2025-11-28 145643" src="https://github.com/user-attachments/assets/ffee384a-2bd8-4843-8f14-e0133b308e9a" />

## AWS EC2 Instance dashboard Instance details

<img width="1905" height="675" alt="Screenshot 2025-11-28 152829" src="https://github.com/user-attachments/assets/8cc9a5c2-53d6-45fb-b5b5-1bb29008ea7d" />

# project folder view Dockerfile + Compose presence

<img width="1920" height="1080" alt="Screenshot (35)" src="https://github.com/user-attachments/assets/32b8fb28-8edd-46c6-80f8-30fc75380476" />
