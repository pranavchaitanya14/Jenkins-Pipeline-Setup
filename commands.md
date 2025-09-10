# Jenkins Pipeline Setup on Azure VM – Commands

This file contains all the step-by-step commands required to set up Jenkins on an Azure VM.

---

## 1. Provision Azure Resources

### 🔹 Login to Azure
Authenticate with your Azure account.
```bash
az login
```

### 🔹 Create Resource Group
A resource group is a logical container in Azure where related resources will be created.
```bash
az group create --name jenkins-rg --location eastus
```

### 🔹 Create Virtual Machine
Provision an Ubuntu VM named **jenkins1** with SSH keys.
```bash
az vm create   --resource-group jenkins-rg   --name jenkins1   --image UbuntuLTS   --admin-username azureuser   --generate-ssh-keys
```

### 🔹 Open Required Ports
Allow traffic for Jenkins (8080), web (80, 443), and SSH (22).
```bash
az vm open-port --resource-group jenkins-rg --name jenkins1 --port 8080
```
```bash
az vm open-port --resource-group jenkins-rg --name jenkins1 --port 80
```
```bash
az vm open-port --resource-group jenkins-rg --name jenkins1 --port 443
```
```bash
az vm open-port --resource-group jenkins-rg --name jenkins1 --port 22
```

---

## 2. Connect to VM
Use SSH to log into your VM. Replace `<Public-IP>` with your VM’s public IP address.
```bash
ssh -i ~/.ssh/id_rsa azureuser@<Public-IP>
```

---

## 3. Install Java & Jenkins

### 🔹 Update Packages
```bash
sudo apt update && sudo apt upgrade -y
```

### 🔹 Install Java
```bash
sudo apt install openjdk-11-jdk -y
java -version
```

### 🔹 Add Jenkins Repo Key & Install Jenkins
```bash
curl -fsSL https://pkg.jenkins.io/debian/jenkins.io.key | sudo tee   /usr/share/keyrings/jenkins-keyring.asc > /dev/null
```
```bash
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]   https://pkg.jenkins.io/debian binary/ | sudo tee   /etc/apt/sources.list.d/jenkins.list > /dev/null
```
```bash
sudo apt update
sudo apt install jenkins -y
```

### 🔹 Start Jenkins
```bash
sudo systemctl enable jenkins
sudo systemctl restart jenkins
sudo systemctl status jenkins -l
```

---

## 4. Unlock Jenkins

### 🔹 Open Jenkins in Browser
```
http://<Public-IP>:8080
```

### 🔹 Get Initial Password
```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

Paste this password into the **Unlock Jenkins** screen.

---

## 5. Customize Jenkins
- Select **Install Suggested Plugins**  
- Create Admin User for Jenkins  
- Install extra plugins if needed  

---

## 6. Install Docker Plugin
1. Go to `Manage Jenkins → Plugin Manager → Available Plugins`  
2. Search for **Docker Pipeline** and install it  

---

## 7. Create a Pipeline
1. Go to Jenkins Dashboard → **New Item**  
2. Select **Pipeline** → Enter project name → Click **OK**  
3. In Pipeline definition, choose **Pipeline script from SCM (Git)**  
4. Paste your GitHub repository URL containing `Jenkinsfile`  
5. Save configuration  

---

## 8. Run the Pipeline
- Click **Build Now**  
- Jenkins will pull code from GitHub and execute pipeline steps  
- Check **Build Console Output** for logs  

---

## ✅ Final Result
Jenkins Pipeline successfully runs from GitHub code on an Azure VM with Docker support.
