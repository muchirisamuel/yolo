IP3 YOLO PROJECT

# Step 1: Playbook Definition
---
- name: IP3 yolo Playbook
  hosts: all
  become: true
 
In this section, I defined the Ansible playbook named "IP3 yolo Playbook." It specifies the target hosts where the tasks will be executed.
The become: true attribute indicates that the playbook tasks will run with root privileges using "sudo."

# Step 2: Task to Install Node.js and npm

  tasks:
    - name: Install Node.js and npm
      apt:
        name: ["nodejs", "npm"]
        state: present

This task installs Node.js and npm on the remote machine using the "apt" package manager. The package names "nodejs" and "npm" are provided as a list, and the "state: present" ensures they are installed.

# Step 3: Task to Install MongoDB
 - name: Install MongoDB
      apt:
        name: mongodb
        state: present
This task installs MongoDB on the remote machine using the "apt" package manager. The package name "mongodb" is provided, and "state: present" ensures MongoDB is installed.

# Step 4: Tasks to Copy Frontend and Backend File
  - name: Copy client files to the server
      copy:
        src: /home/sam/Documents/Moringa/IPs/yolo/client/
        dest: /home/vagrant/client

    - name: Copy client files to the server
      copy:
        src: /home/sam/Documents/Moringa/IPs/yolo/backend/
        dest: /home/vagrant/backend
These tasks copy the client and backend application files from the control node (local machine) to the vagrant machine. 

# Step 5: Tasks to Install npm Packages for client and backend

    - name: Install npm packages for client
      command: npm install
      args:
        chdir: /home/vagrant/client

    - name: Install npm packages for backend
      command: npm install
      args:
        chdir: /home/vagrant/backend

These tasks install npm packages for the frontend and backend applications on the remote machine. The "npm install" command is executed within the specified directories using the "chdir" parameter.

# Step 6: Task to Start Backend Server

    - name: Start backend server
      command: npm start
      args:
        chdir: /home/vagrant/backend

This task starts the backend server on the remote machine using the "npm start" command. The "chdir" parameter ensures that the command is executed within the backend application directory.