# Jenkins Pipeline Setup - Screenshots Explanation

This document provides explanations for each screenshot captured during the Jenkins setup on an Azure VM.

---

## 📸 Screenshot 1: Azure Login
![Azure Login](images/screenshot1.png)

**Command Shown:**
```bash
az login
```

**Explanation:**
Authenticates your local machine with Azure. Once logged in, you can start creating and managing Azure resources from the CLI.

---

## 📸 Screenshot 2: SSH Key Generation
![SSH Keygen](images/screenshot2.png)

**Command Shown:**
```bash
ssh-keygen
```

**Explanation:**
Generates an SSH key pair (`id_rsa` and `id_rsa.pub`):
- The private key stays on your machine.
- The public key is used for secure authentication with the Azure VM.

---

## 📸 Screenshot 3: SSH into VM
![SSH into VM](images/screenshot3.png)

**Command Shown:**
```bash
ssh -i ~/.ssh/id_rsa azureuser@<Public-IP>
```

**Explanation:**
This connects your local system to the Azure VM using the SSH private key.
- `azureuser` → VM admin username.
- `<Public-IP>` → Public IP assigned to the VM.

---

## 📸 Screenshot 4: Install Java and Jenkins
![Install Jenkins](images/screenshot4.png)

**Commands Shown:**
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install openjdk-11-jdk -y
java -version

sudo apt install jenkins -y
sudo systemctl enable jenkins
sudo systemctl restart jenkins
sudo systemctl status jenkins -l
```

**Explanation:**
- Updates system packages.  
- Installs **Java** (required for Jenkins).  
- Installs **Jenkins** and enables it as a service.  
- Starts Jenkins so it is accessible on port `8080`.  

---

## 📸 Screenshot 5: Jenkins Initial Admin Password
![Initial Admin Password](images/screenshot5.png)

**Command Shown:**
```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

**Explanation:**
- Retrieves the **initial Jenkins admin password**.  
- This password is required when unlocking Jenkins in the browser at:
  ```
  http://<Public-IP>:8080
  ```
- After entering it, you proceed with setup → install suggested plugins → create admin user → configure Jenkins.

---

✅ These screenshots illustrate the full workflow:  
1. Azure Login  
2. SSH Key Generation  
3. Connect to VM  
4. Install Jenkins  
5. Retrieve Jenkins Initial Password  
