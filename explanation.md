# Kubernetes Deployment Explanation
## 1. Kubernetes Objects

### MongoDB Database
- **StatefulSet**:
  - StatefulSets maintain the state of MongoDB across pod restarts and rescheduling
  - This approach ensures data consistency in distributed database environments

- **Service (Headless)**: A headless service is used for MongoDB to provide network identity to each pod while allowing direct communication to specific instances

### Backend API
- **Deployment**: For the backend service, a standard Deployment is appropriate because:
  - The backend is stateless and can be scaled horizontally
  - Any backend instance can handle requests without needing persistent identity
  - Rolling updates can be easily managed

- **Service**: LoadBalancer service to ensure the browser is able to access it when user is interacting with client

### Frontend Web Application
- **Deployment**: Similar to the backend, the frontend is stateless and benefits from:
  - Horizontal scaling capabilities
  - Simplified rolling updates and rollbacks
  - Load balancing across multiple pods

- **Service**: LoadBalancer service to expose the frontend to external traffic


## 2. Method Used to Expose Pods to Internet Traffic

I've chosen different exposure methods for different components:

1. **Frontend** - LoadBalancer Service:
   - Creates a GCP Load Balancer with an external IP automatically
   - Provides direct internet access to the frontend application

2. **Backend** - LoadBalancer Service:
   - Since we are not using any backend proxy for client the service has to be exposed externaly via  a loadbalancer.
   - This will allow the browser to reolve the IP when client is interacting with backend.


## 3. Use of Persistent Storage

### MongoDB StatefulSet with Persistent Volumes
- **volumeClaimTemplates**: Each MongoDB pod gets its own PersistentVolumeClaim


### Backend and Frontend
- These components are stateless and don't require persistent storage
- All state is maintained in the database
- This enables easy scaling and replacement of these pods

### Volume Types Used
- **PersistentVolumeClaim**: For MongoDB data

This approach ensures that critical data is preserved while maintaining the flexibility of Kubernetes orchestration.
