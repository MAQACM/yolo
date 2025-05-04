# Ansible Playbook Explanation



## Order of Execution

The playbook is executed sequentially, following a logical progression from basic system setup to deploying the complete application. The order is important because each step builds upon the previous ones:

1. **Common Role** - This role sets up the basic system environment, installs required packages, and clones the application repository. It must run first since all subsequent roles depend on these prerequisites.

2. **Docker Role** - After setting up the basic system, we install Docker and Docker Compose. This is necessary before deploying any containers, so it must come before the application-specific roles.

3. **MongoDB Role** - The database is deployed first because both the backend and frontend depend on it. By establishing the database first, we ensure it's available when the application services start.

4. **Backend Role** - The backend container is deployed after the database. The backend needs to connect to the database and must be available before the frontend can make API calls to it.

5. **Frontend Role** - The frontend container is deployed last in the application stack, as it depends on the backend API being available.

6. **Deploy Role** - This role uses docker-compose to ensure all containers are properly networked and running together. It serves as a verification step to make sure everything is operational.

## Role Functions and Ansible Modules Used

### Common Role
- **Function**: Prepares the base system with all required dependencies and clones the repository.
- **Modules Used**:
  - `apt`: Installs required system packages
  - `file`: Creates directories for the application
  - `git`: Clones the application repository
  - `pip`: Installs Python dependencies

### Docker Role
- **Function**: Installs Docker and Docker Compose, creates a Docker network for container communication.
- **Modules Used**:
  - `apt`: Removes old Docker versions and installs new Docker packages
  - `apt_key`: Adds Docker's GPG key for package verification
  - `apt_repository`: Adds the Docker repository
  - `group`: Creates Docker group
  - `user`: Adds the vagrant user to the Docker group
  - `command`: Checks Docker Compose version and also create docker network
  - `get_url`: Downloads Docker Compose
  - `systemd`: Ensures Docker service is running

### MongoDB Role
- **Function**: Sets up the MongoDB container for data persistence.
- **Modules Used**:
  - `file`: Creates data directory for MongoDB
  - `command`: Checks if MongoDB container exists and also pull docker image,used in place of docker_image

### Backend Role
- **Function**: Builds and runs the backend API container.
- **Modules Used**:
  - `file`: validates the backend repository exists
  - `docker_image`: Builds the backend Docker image

### Frontend Role
- **Function**: Builds and runs the frontend web container.
- **Modules Used**:
  - `file`: Ensures client directory exists
  - `docker_image`: Builds the frontend Docker image

### Deploy Role
- **Function**: Ensures all containers are running together using docker-compose.
- **Modules Used**:
  - `copy`: Copies the docker-compose file
  - `community.docker.docker_compose_v2`: Deploys the application using docker-compose v2
  - `command`: Checks running containers
  - `uri`: Verifies application endpoints
  - `debug`: Displays application status
