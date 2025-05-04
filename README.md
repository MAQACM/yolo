# Yolomy E-commerce Ansible Automation

This project automates the deployment of the Yolomy e-commerce platform using Ansible for configuration management and Docker for containerization.

## Project Structure

```
.
├── Vagrantfile
├── playbook.yml
├── vars
│   └── main.yml
├── roles
│   ├── common
         └──tasks
             └──main.yml
│   ├── docker
          └──tasks
             └──main.yml
│   ├── mongodb
          └──tasks
             └──main.yml
│   ├── backend
          └──tasks
             └──main.yml
│   ├── frontend
          └──tasks
             └──main.yml
│   └── deploy
          └──tasks
             └──main.yml
├── README.md
└── explanation.md
```

## Prerequisites

- Vagrant
- VirtualBox
- Ansible
- Ansible-galaxy 

## Getting Started

1. Clone this repository:
   ```
   git clone hhttps://github.com/MAQACM/yolo.git
   cd yolo
   ```

2. Start the Vagrant VM:
   ```
   vagrant up
   ```
   This will provision the VM and run the Ansible playbook automatically.

3. Access the application:
   - Frontend: http://192.168.56.10:3000
   - Backend API: http://192.168.56.10:4000/api/products

## Features

- Fully automated deployment of a three-tier e-commerce application
- Containerized architecture with MongoDB, Node.js backend, and React frontend
- Data persistence with Docker volumes
- Configurable through Ansible variables
- Organized with Ansible roles for modularity and reusability

## Manual Execution

If you need to run the playbook manually:

```
ansible-playbook -i inventory playbook.yml
```

To run specific parts using tags:

```
ansible-playbook -i inventory playbook.yml --tags "docker,mongodb"
```

## Role Descriptions
See `explanation.md` for detailed information about the execution order and Ansible modules used.

## Testing

To verify the application is running correctly:

1. Navigate to http://192.168.56.10:3000 in your browser
2. Try adding a product through the provided form
3. Verify the product appears in the product listing by refreshing the page after addation

## Troubleshooting

- **Container issues**: Check container status with `docker ps -a` inside the VM
- **Log inspection**: View logs with `docker logs <container_name>`
- **Network problems**: Verify connectivity with `docker network inspect app-net`
- **Database issues**: Connect to MongoDB with `docker exec -it app-mongo mongosh`
- **VBoxManage:error:VERR_VMX_IN_VMX_ROOT_MODE**: Unload KVM modules ` sudo lsof /dev/kvm sudo kill -9 <PID> sudo modprobe -r kvm_intel sudo modprobe -r kvm` or reboot as a last result
