# Kubernetes Application Deployment

This repository contains Kubernetes manifests for deploying the Mark Yolo application (frontend, backend, MongoDB) on Google Kubernetes Engine (GKE).

## Project Overview

This project deploys a containerized application consisting of:
- A React frontend client (mark-yolo-client)
- A Node.js backend API (mark-yolo-backend)
- A MongoDB database with persistent storage using StatefulSets (app-ip-mongo)

## HLD

```
┌─────────────────┐      ┌─────────────────┐      ┌─────────────────┐
│                 │      │                 │      │                 │
│  mark-yolo-     │      │  mark-yolo-     │      │  app-ip-mongo   │
│  client         │──────▶  backend        │──────▶  StatefulSet    │
│  (LoadBalancer) │      │  (LoadBalancer) │      │  (Headless Svc) │
│                 │      │                 │      │                 │
└─────────────────┘      └─────────────────┘      └─────────────────┘
       ▲                                                 │
       │                                                 │
       │                                                 ▼
┌──────┴────────┐                               ┌─────────────────┐
│  Internet     │                               │  Persistent     │
│  (HTTPS/HTTP) │                               │  Volume         │
└───────────────┘                               └─────────────────┘
```

**Key components:**
- MongoDB StatefulSet for data persistence
- Backend API Deployment
- Frontend Web Deployment
- Various Services for networking

## Prerequisites

- Google Cloud SDK installed
- Access to a GKE cluster
- Enable kubernetes API
- kubectl configured to work with your GKE cluster
- Docker Hub account (for storing your container images)

## Manual Deployment Steps

1. **Clone this repository:**
   ```bash
   git clone https://github.com/MAQACM/yolo.git
   cd manifests
   ```

2. **Create a GKE cluster:**
   ```bash
   gcloud container clusters create yolo-cluster \
     --zone us-central1-a \
     --disk-type=pd-standard \
     --machine-type=e2-medium
   ```

3. **Get authentication credentials:**
   ```bash
   gcloud container clusters get-credentials yolo-cluster --zone us-central1-a
   ```

4. **Create a namespace:**
   ```bash
   kubectl create namespace yolo-app
   ```

5. **Deploy MongoDB StatefulSet:**
   ```bash
   kubectl create -f mongodb-deployment.yaml -n yolo-app
   ```

6. **Deploy Backend:**
   ```bash
   kubectl create -f backend-deployment.yaml -n yolo-app
   ```

7. **Deploy Frontend:**
   ```bash
   kubectl create -f frontend-deployment.yaml -n yolo-app
   ```

8. **Get the External IP:**
   ```bash
   kubectl get service mark-yolo-client -n yolo-app
   ```
   The output will include an EXTERNAL-IP column.

## Accessing the Application
Current deployment URL: `http://35.247.49.1`

## Project Structure

```
└── manifests            
 ├── mongo-deployment.yaml    
 ├── backend-deployment.yaml      
 ├── frontend-deployment.yaml  
├── explanation.md      
└── README.md                   
```

## Implementation Details

For a detailed explanation of the implementation choices,refer to the [explanation.md](explanation.md) file.

