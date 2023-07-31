IP3 YOLO PROJECT

# Step 1: Playbook Definition

---
- name: IP3 yolo Playbook
  hosts: all
  become: true
 
In this section, I defined the Ansible playbook named "IP3 yolo Playbook." It specifies the target hosts where the tasks will be executed.
The become: true attribute indicates that the playbook tasks will run with root privileges using "sudo."

# Step 2: Task to Update apt Cache and Install Git

  tasks:
    - name: Update apt cache
      apt:
        update_cache: yes

    - name: Install Git
      apt:
        name: git
        state: present


These tasks update the apt cache on the virtual machine to ensure it has the latest package information. Then, the playbook installs Git using the "apt" package manager so that the repository can be cloned from GitHub.

# Step 3: Task to Clone the Full-stack Web App from GitHub

    - name: Cloning the Full-stack Web App from GitHub
      git:
        repo: https://github.com/Vinge1718/yolo
        dest: /home/vagrant
        version: master

This task clones the full-stack web application from the specified GitHub repository (yolo) to the remote virtual machine.  

# Step 4: Task to Install Node.js and npm

    - name: Install Node.js and npm
      apt:
        name: ["nodejs", "npm"]
        state: present

This task installs Node.js and npm on the virtual machine using the "apt" package manager.

# Step 5: Task to Install MongoDB and Allow Incoming Traffic on Ports 5000 and 3000

    - name: Install MongoDB
      apt:
        name: mongodb
        state: present

    - name: Allow incoming traffic on port 5000 (frontend)
      ufw:
        rule: allow
        port: 5000

    - name: Allow incoming traffic on port 3000 (backend)
      ufw:
        rule: allow
        port: 3000

    - name: Enable ufw firewall
      ufw:
        state: enabled

These tasks install MongoDB on the virtual machine using the "apt" package manager. Additionally, they allow incoming traffic on ports 5000 and 3000 using the ufw module, which manages the firewall on Ubuntu. The last task enables the firewall.

# Step 6: Task to Start MongoDB Service

    - name: Start MongoDB service
      service:
        name: mongod
        state: started

This task starts the MongoDB service on the virtual machine so that your web application can interact with the database

# Step 7: Tasks to Install npm Dependencies and Start Frontend and Backend Servers
    - name: Install npm dependencies for frontend
      command: npm install
      args:
        chdir: /home/vagrant/yolo/client

    - name: Install npm dependencies for backend
      command: npm install
      args:
        chdir: /home/vagrant/yolo/backend

    - name: Start frontend server
      command: npm start
      args:
        chdir: /home/vagrant/yolo/client

    - name: Start backend server
      command: npm start
      args:
        chdir: /home/vagrant/yolo/backend

# Step 8: Adds a mongodb user

    - name: Add MongoDB user
      mongo_user:
        login_user: admin
        login_password: password
        user: bob
        password: 12345
        roles: readWrite

    - name: Create MongoDB database
      mongo_db:
        login_user: bob
        login_password: 12345
        name: appdb

