# Linux Environment Set Up
This consists of the begineer friendly guide to set up lux environment to the advanced devops level projects..
#  DevOps Lab Setup: Windows → WSL2 → Ubuntu → Docker

##  Overview

In the last few hours, I set up a complete DevOps-ready Linux environment on my Windows machine.

Instead of running Ubuntu inside Docker on Windows (which causes path, mount, and shell issues), I built a clean and production-like setup using:

Windows → WSL2 → Ubuntu 22.04 → Docker Engine

This environment now allows me to:

* Practice Linux commands
* Build and run Docker containers
* Simulate real DevOps workflows
* Work in a production-like Linux shell

---

## Why This Approach?

Initially, I attempted to run Ubuntu using Docker on Windows.
However, I faced issues like:

* CMD vs PowerShell command differences
* Path mounting problems
* Special character issues in Windows username
* Multi-line command execution failures

After troubleshooting, I switched to a more professional setup using WSL2.

This eliminated:

* Windows path conversion problems
* Shell inconsistencies
* Docker Desktop dependency issues
* Environment instability

---

##  Step-by-Step Setup Process

### 1️ Installed WSL2

```powershell
wsl --install
```

Verified installation:

```powershell
wsl --status
```

---

### 2️ Installed Ubuntu 22.04 LTS

```powershell
wsl --install -d Ubuntu-22.04
```

Created a UNIX user:

```
username: abhishek
```

Verified installation:

```bash
lsb_release -a
```

---

### 3️ Updated Ubuntu System

```bash
sudo apt update && sudo apt upgrade -y
```

---

### 4️ Installed Docker Engine (Inside Ubuntu)

Installed required packages:

```bash
sudo apt install -y ca-certificates curl gnupg lsb-release
```

Added Docker GPG key:

```bash
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

Added Docker repository:

```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

Installed Docker:

```bash
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

Enabled Docker without sudo:

```bash
sudo usermod -aG docker $USER
exit
```

---

##  Verification

Checked Docker version:

```bash
docker --version
```

Output:

```
Docker version 29.2.1
```
https://github.com/ImAbhi56/Linux-ultimate-guide/blob/f61be933e927b4dfa8a60272a695f521b33e8be0/docker%20version%20check.png 

Tested Docker:

```bash
docker run hello-world
```

Successfully received:

```
Hello from Docker!
```


---

##  Ran First Production-Style Container

After verifying Docker installation using `hello-world`, I deployed a real web server container using Nginx.

###  Run Nginx Container

```bash
docker run -d -p 8080:80 --name mynginx nginx
```

* `-d` → Run in detached mode
* `-p 8080:80` → Map container port 80 to host port 8080
* `--name mynginx` → Assign readable container name

---
https://github.com/ImAbhi56/Linux-ultimate-guide/blob/7adfb43d91627694756f76000e30ed3658d392e2/docker%20run%20nginx.png 

###  Verify Running Container

```bash
docker ps
```

Output showed port mapping:

```
0.0.0.0:8080->80/tcp
```

This confirmed that the container was accessible from Windows.

---
https://github.com/ImAbhi56/Linux-ultimate-guide/blob/72758e89b21617c7a24a45eb98a71f1261d5cf5b/pulling%20nginx%20image.png 

###  Access From Browser

Opened:

```
http://localhost:8080
```

Successfully loaded the Nginx welcome page.

This confirmed:

* Container networking works
* Port forwarding works
* WSL ↔ Windows integration works

---
https://github.com/ImAbhi56/Linux-ultimate-guide/blob/98e1092bb5818d265620f3661eacd69986e10567/Nginx%20running.png 

###  Stop and Remove Container

```bash
docker stop mynginx
docker rm mynginx
```
https://github.com/ImAbhi56/Linux-ultimate-guide/blob/aee5c8fe864a18df7a9d8f142b0d455a7abf6605/docker%20file%20stop.png 

This completed the container lifecycle.

---

##  Key Concepts Learned

* Docker image vs container
* Detached mode execution
* Port mapping (`host:container`)
* Container naming
* Viewing running containers
* Stopping and removing containers
* Basic container lifecycle management


##  Final Architecture

```
Windows
   ↓
WSL2 (Linux Kernel)
   ↓
Ubuntu 22.04
   ↓
Docker Engine
   ↓
Containers
```

This setup mirrors how many DevOps engineers work in real-world environments.

---

##  Key Learnings

* Difference between CMD, PowerShell, and Linux shell
* Why WSL2 is better than running Ubuntu directly in Docker
* How Docker daemon and client interact
* How container lifecycle works
* How to expose container ports
* Why Linux-native Docker is more stable for DevOps practice

##  Reflection

This exercise reinforced the importance of:

* Choosing the right architecture
* Debugging environment issues
* Understanding system internals instead of blindly copying commands

Now I have a clean, production-grade DevOps lab ready for hands-on practice.
