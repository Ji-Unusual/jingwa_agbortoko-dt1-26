# Data Engineering / Software Deployment Project (`jingwa_agbortoko-dt1-26`)

**GitHub Repository:** `Ji-Unusual/jingwa_agbortoko-dt1-26`  
**GitHub Username:** `Ji-Unusual`  
**Docker Hub Namespace:** `thejii`  

---

## 🛠️ Overview & Deployment

This project consists of a containerized application featuring a Python-based API service and a Streamlit frontend UI deployed on a Google Cloud Platform (GCP) Virtual Machine using Docker and Docker Compose.

### Docker Hub Repositories
* **API Service Image (Private):** `thejii/dt1-api:latest`
* **Frontend Service Image (Public):** `thejii/dt1-frontend:latest`

### Running the Application Locally / VM Setup
To build and run the services using Docker Compose:

```bash
docker compose up --build -d

## 🏗️ System Architecture

Below is the architecture diagram illustrating the components, how they interact, and how data flows through the deployed system:

![Architecture Diagram](./Architecture%20diagram.jpg)

### Component Interaction & Data Flow
1. **User Access:** External users access the frontend service via their web browser on the GCP VM's external IP address through **Port 8501** (`http://<GCP_VM_EXTERNAL_IP>:8501`).
2. **Frontend Container (`dt1-frontend`):** Built with Streamlit, this container serves the UI. It makes HTTP requests to fetch data from the API backend.
3. **Backend Container (`dt1-api`):** A Python-based API service listening on **Port 5001**.
4. **Internal Communication:** Docker Compose establishes an internal bridge network, allowing the frontend to address the API directly as `http://api:5001/` via container service hostname resolution.
5. **Data Management:** The API processes incoming requests, querying the necessary files/embedded storage, and returns JSON payloads back to the frontend to render.

---

## ⚠️ System Shortcomings & Future Improvements

### 1. Lack of HTTPS / TLS Security
* **Shortcoming:** Web traffic and API calls currently travel in unencrypted HTTP.
* **Improvement:** Introduce a reverse proxy (such as Nginx or Caddy) with automated TLS/HTTPS certificates via Let’s Encrypt.

### 2. Embedded / Local Storage Volatility
* **Shortcoming:** Storage relies on local VM storage rather than a dedicated cloud database.
* **Improvement:** Migrate databases to fully managed cloud database services (e.g., Google Cloud SQL) with persistent backups.

### 3. Direct Port Exposure
* **Shortcoming:** Ports 8501 and 5001 are bound directly to VM ports.
* **Improvement:** Keep container ports strictly private inside the Docker virtual network, routing external traffic exclusively through an API Gateway or Nginx reverse proxy on port 80/443.

### 4. Scalability Boundaries
* **Shortcoming:** Both containers share resources on a single GCP VM, creating a single point of failure under heavy workload spikes.
* **Improvement:** Deploy on container orchestrators like Google Kubernetes Engine (GKE) or serverless container platforms like Google Cloud Run.
