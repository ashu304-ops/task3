
#  Dockerized Web App Deployment using Ansible Playbook on Kubernetes Cluster

This project demonstrates a complete CI/CD workflow for deploying a **React.js application** using **Docker**, **Ansible**, and **Kubernetes (Minikube)** — all orchestrated from your local machine to a remote host.

---

##  Project Structure

```
project/
├── ansible/
│   ├── inventory.ini                   # Target host info
│   ├── playbooks/
│   │   ├── install.yaml                # Installs Docker & Minikube
│   │   ├── start.yaml                  # Starts/Resets Minikube
│   │   └── deploy.yaml                 # Builds & deploys app
│   └── roles/
│       ├── deployment.yaml             # K8s deployment manifest
│       └── service.yaml                # K8s service manifest
├── app/
│   ├── Dockerfile                      # Multi-stage Dockerfile
│   ├── public/                         # Static files
│   ├── src/
│   │   ├── components/
│   │   │   ├── App.jsx
│   │   │   └── dict.jsx
│   │   ├── emojipedia.js
│   │   ├── index.jsx
│   ├── package.json
│   └── vite.config.js
```

---

## Tech Stack

* **Frontend**: React.js (Vite)
* **CI/CD Automation**: Ansible
* **Containerization**: Docker
* **Orchestration**: Kubernetes (Minikube)
* **Web Server**: Nginx

---

##  Ansible Playbooks

###  `install.yaml`

Installs required dependencies on the **remote host**:

* Docker
* Minikube
* Kubectl

---

###  `start.yaml`

Starts a **clean Minikube cluster** using the Docker driver and ensures a fresh setup for every deployment.

---

###  `deploy.yaml`

* Builds the React Docker image on your **local machine**
* Transfers the image to the **remote host**
* Loads the image into **Minikube**
* Deploys via Kubernetes manifests (`deployment.yaml`, `service.yaml`)

---

##  Dockerfile (Multi-Stage)

1. **Build Stage**: Compiles the React app using `node:latest`.
2. **Production Stage**: Serves the compiled static files using `nginx:alpine`.

>  Optimized for small image size and performance.

---

##  Kubernetes Configuration

### `deployment.yaml`

* Deploys a single pod using image `react-app:v1`
* Container listens on port `80`

### `service.yaml`

* Exposes app using **NodePort** (`30036`)
* Type: `LoadBalancer` (for Minikube exposure)

---

##  React Application

A minimalist **emoji dictionary** web app powered by:

* `App.jsx`: Main component rendering emoji entries
* `dict.jsx`: Individual emoji renderer
* `emojipedia.js`: Static emoji data source

---

## 🚀 Deployment Instructions

### 1.  Configure Ansible Inventory

Update `ansible/inventory.ini` with your remote server details:

```ini
[web]
192.168.1.X ansible_user=your_user ansible_ssh_private_key_file=~/.ssh/id_rsa ansible_port=2222
```

---

### 2.  Install Dependencies on Remote Server

```bash
cd ansible/playbooks
ansible-playbook -i ../inventory.ini install.yaml
```

---

### 3.  Start Minikube Cluster

```bash
ansible-playbook -i ../inventory.ini start.yaml
```

---

### 4.  Build and Deploy the App

```bash
ansible-playbook -i ../inventory.ini deploy.yaml
```

---

### 5.  Access the Application

Find the Minikube IP:

```bash
minikube ip
```

Then access your app at:

```
http://<Minikube-IP>:30036
```

---

##  Additional Notes

* You **build the image locally** and transfer it to the remote server using Ansible and `docker save/load`.
* Kubernetes manifests are stored under `ansible/roles/`.
* The app uses **Vite** for fast build and **Nginx** for production-grade static serving.

---

##  Future Improvements

* Add GitHub Actions for full remote CI/CD
* Use Ingress controller instead of NodePort
* Add TLS (Let's Encrypt via Cert Manager)
* Extend support for multi-node Kubernetes

---

