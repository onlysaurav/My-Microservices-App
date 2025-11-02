# 🚀 Node.js Microservices Containerization using Docker & Docker Compose

This project demonstrates how to **containerize a microservices-based Node.js application** and orchestrate multiple services using **Docker Compose**.

---

## 📘 Project Overview

This application is composed of four independent microservices:

| Service | Description | Port |
|----------|--------------|------|
| **User Service** | Handles user-related APIs | 3000 |
| **Product Service** | Manages product data | 3001 |
| **Order Service** | Handles order creation and management | 3002 |
| **Gateway Service** | Acts as an API gateway routing requests to respective services | 3003 |

Each service runs in its own **Docker container** and communicates internally through a **Docker network** created by Compose.

---

## ⚙️ Prerequisites

Before starting, ensure the following are installed:

- [Docker Desktop](https://www.docker.com/products/docker-desktop/)
- [Docker Compose](https://docs.docker.com/compose/)
- [Git](https://git-scm.com/)
- Node.js (optional, only if you want to test locally before containerizing)

---

## 🧩 Folder Structure

```

submission/
├── user-service/
│   ├── Dockerfile
│   ├── package.json
│   └── server.js
├── product-service/
│   ├── Dockerfile
│   ├── package.json
│   └── server.js
├── order-service/
│   ├── Dockerfile
│   ├── package.json
│   └── server.js
├── gateway-service/
│   ├── Dockerfile
│   ├── package.json
│   └── server.js
├── docker-compose.yml
└── README.md

````

---

## 🏗️ Setup Instructions

### **1️⃣ Clone the Repository**
```bash
git clone https://github.com/onlysaurav/My-Microservices-App.git
cd My-Microservices-App/Microservices
````

### **2️⃣ Verify Directory Structure**

Ensure that each microservice (`user-service`, `product-service`, etc.) contains:

* a `Dockerfile`
* a `package.json`
* a `server.js` or app entry file

Example:

```
user-service/
  ├── Dockerfile
  ├── package.json
  └── server.js
```

---

## 🐳 Docker Setup

### **3️⃣ Build and Start All Containers**

```bash
docker compose up --build
```

This will:

* Build Docker images for all microservices using their Dockerfiles
* Create a shared bridge network for internal communication
* Start all containers automatically

### **4️⃣ Verify Running Containers**

Run:

```bash
docker ps
```

You should see something like:

```
CONTAINER ID   IMAGE                        STATUS          PORTS
abcd1234       microservices-user-service   Up 2 minutes    0.0.0.0:3000->3000/tcp
abcd5678       microservices-product-service Up 2 minutes   0.0.0.0:3001->3001/tcp
abcd9101       microservices-order-service   Up 2 minutes   0.0.0.0:3002->3002/tcp
abcd1121       microservices-gateway-service Up 2 minutes   0.0.0.0:3003->3003/tcp
```

---

## 🧪 How to Test Each Service

Once the containers are running, test each service using your browser or `curl`:

| Service         | Endpoint                                       | Test Command                 |
| --------------- | ---------------------------------------------- | ---------------------------- |
| User Service    | [http://localhost:3000](http://localhost:3000) | `curl http://localhost:3000` |
| Product Service | [http://localhost:3001](http://localhost:3001) | `curl http://localhost:3001` |
| Order Service   | [http://localhost:3002](http://localhost:3002) | `curl http://localhost:3002` |
| Gateway Service | [http://localhost:3003](http://localhost:3003) | `curl http://localhost:3003` |

Each should return a basic success message like:

```
User Service Running
```

---

## 🧰 Docker Commands Reference

| Purpose             | Command                     |
| ------------------- | --------------------------- |
| Build images        | `docker-compose build`      |
| Start containers    | `docker-compose up`         |
| Start in background | `docker-compose up -d`      |
| Stop containers     | `docker-compose down`       |
| View logs           | `docker-compose logs -f`    |
| Rebuild and restart | `docker-compose up --build` |

---

## 🩺 Basic Troubleshooting Tips

| Problem                           | Possible Cause                      | Solution                                                                   |
| --------------------------------- | ----------------------------------- | -------------------------------------------------------------------------- |
| `Cannot connect to Docker daemon` | Docker Engine not running           | Start Docker Desktop or run `sudo systemctl start docker` (Linux)          |
| `port already in use`             | Another process using the same port | Change port in `docker-compose.yml` or stop that process                   |
| Containers exit immediately       | Missing or wrong CMD in Dockerfile  | Ensure last line in Dockerfile: `CMD ["node", "server.js"]`                |
| Code changes not showing          | Old image cached                    | Run: `docker-compose up --build`                                           |
| One service not reachable         | Wrong service name or network issue | Check service name in `docker-compose.yml` and confirm all in same network |
| “Permission denied” error         | File permissions                    | Run `chmod -R 755 <service-folder>` before build                           |

---

## 📸 Screenshots

**1️⃣ Docker Containers Running**
<img width="1280" height="800" alt="Screenshot 2025-11-02 at 10 59 34 AM" src="https://github.com/user-attachments/assets/84c98e6a-a4eb-4847-a9f5-468fd74e7c7c" />

<img width="1280" height="800" alt="Screenshot 2025-11-02 at 11 03 46 AM" src="https://github.com/user-attachments/assets/ae3c8785-544c-461a-9a1b-a408778547c8" />

<img width="1280" height="800" alt="Screenshot 2025-11-02 at 11 04 04 AM" src="https://github.com/user-attachments/assets/8080a43c-c241-4e4c-82d3-6d82a6ca593c" />

<img width="1280" height="800" alt="Screenshot 2025-11-02 at 11 25 21 AM" src="https://github.com/user-attachments/assets/bbc11761-fa89-4698-aea5-5a7797e1d62c" />

---

## 🧾 Example Dockerfile (for any Node.js service)

```dockerfile
# Use an official Node.js runtime as a parent image
FROM node:18-alpine

# Set working directory
WORKDIR /app

# Copy package.json and install dependencies
COPY package*.json ./
RUN npm install --production

# Copy rest of the application code
COPY . .

# Expose the service port
EXPOSE 3000

# Start the service
CMD ["node", "server.js"]
```

---

## 🧩 Example docker-compose.yml

```yaml
services:
  user-service:
    build: ./user-service
    ports:
      - "3000:3000"

  product-service:
    build: ./product-service
    ports:
      - "3001:3001"

  order-service:
    build: ./order-service
    ports:
      - "3002:3002"

  gateway-service:
    build: ./gateway-service
    ports:
      - "3003:3003"
    depends_on:
      - user-service
      - product-service
      - order-service
```

---

## 🏁 Conclusion

This project successfully demonstrates:

* **Dockerfile creation** for each service
* **Docker Compose orchestration** for multi-container setup
* **Local testing and accessibility** via exposed ports
* **Documentation and troubleshooting** for setup validation

You can easily extend this setup to include databases (MongoDB, MySQL, Redis) or deploy it to cloud services like AWS ECS or Kubernetes later.

---

**Author:** Saurabh Tiwari
**GitHub:** [onlysaurav](https://github.com/onlysaurav)


