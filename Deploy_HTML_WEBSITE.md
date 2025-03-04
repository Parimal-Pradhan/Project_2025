# Automating Deployment of index.html on Ubuntu EC2 using GitHub Webhook, Jenkins, and Apache (Pipeline as Code)


To **deploy a static website on an EC2 instance (Ubuntu) using Jenkins**, follow these steps:

## **Overview**
- **User pushes code** to **GitHub**.
- **GitHub Webhook** triggers **Jenkins pipeline**.
- **Jenkins pulls code**, deploys it to **EC2's web server**.

---

## **Prerequisites**
1. **EC2 Ubuntu Instance**
   - Allow **HTTP (Port 80)** and **SSH (Port 22)** in the Security Group.

2. **Install Java**
   sudo apt update
   sudo apt install fontconfig openjdk-17-jre
   java -version
   
3. **Install Jenkins**
  
  sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
  echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
  sudo apt-get update
  sudo apt-get install jenkins
   - Access Jenkins: `http://<EC2-Public-IP>:8080`

4. **Install Git**
  
   sudo apt install git -y
 

5. **Install Web Server (Apache)**
  
   sudo apt install apache2 -y
   sudo systemctl enable apache2
   sudo systemctl start apache2


6. **Install Jenkins Plugins**
   - Go to **Manage Jenkins** → **Plugins**.
   - Install:
     - **Pipeline**
     - **Git Plugin**
     - **GitHub Integration Plugin**

---

## **Step 1: Configure GitHub Webhook**
1. Go to **GitHub Repository** → **Settings** → **Webhooks**.

   ![webhook1](https://github.com/user-attachments/assets/84e74a52-90d4-4943-9f8a-8dfab362eea9)

3. Click **Add Webhook**.
   
![webhook2](https://github.com/user-attachments/assets/480e9f21-748c-4da8-b81e-bbdbadb792fe)
   
5. Set **Payload URL** as:
   ```
   http://<Jenkins-Public-IP>:8080/github-webhook/
   ```

6. Select **Content type**: `application/json`.
7. Choose **Just the push event** → Click **Add Webhook**.

![webhook3](https://github.com/user-attachments/assets/57a51289-3b26-414a-bd25-c12756a09690)


---

## **Step 2: Create a Jenkins Pipeline Job**
1. Open **Jenkins Dashboard** → Click **New Item**.
   ![pipe1](https://github.com/user-attachments/assets/7d429628-4c39-4e63-b400-1c6b231a7efb)

3. Enter **Job Name** (e.g., `Static-Site-Deploy`).
![pipe2](https://github.com/user-attachments/assets/ec2c1561-b492-4d3b-b414-35e03fb2c381)

   
5. Select **Pipeline** → Click **OK**.
![pipe3](https://github.com/user-attachments/assets/ad799fab-bb3e-4e6f-85f0-7944a4fe4703)

---

## **Step 3: Create the Jenkinsfile (Pipeline Script)**
- In the **GitHub repository**, create a file named **Jenkinsfile**.
- Add the following pipeline script:

![1](https://github.com/user-attachments/assets/09bedfe4-736d-4604-ba32-e5c20815f79c)

pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/Parimal-Pradhan/cafe_web.git'
            }
        }

        stage('Deploy to Apache') {
            steps {
                sh '''
                sudo rm -rf /var/www/html/*
                sudo cp -r * /var/www/html/
                sudo systemctl restart apache2
                '''
            }
        }
    }
}
```

---

## **Step 4: Configure Jenkins Pipeline**
1. In **Jenkins Job**, scroll to **Pipeline** section.
2. Select **Pipeline Script from SCM**.
3. Choose **Git**.
4. Enter **GitHub Repo URL**:  

   https://github.com/your-repo/static-website.git](https://github.com/Parimal-Pradhan/![pipe2](https://github.com/user-attachments/assets/52cb5569-12ac-4a0b-8e36-82db26761afe)
cafe_web.git
   
5. Set **Branch**: `main`
![pipe4](https://github.com/user-attachments/assets/dbd7a786-728c-450d-95a0-fa55f874a394)


6. Define **Script Path**: `Jenkinsfile`
7. Click **Save**.
![pipe5](https://github.com/user-attachments/assets/75006a8e-8229-42ef-952a-cc69a4a8b86f)



---

## **Step 5: Trigger Deployment**
1. **Push Code** to GitHub.
   git push origin main
2. **Webhook triggers Jenkins**.
3. **Jenkins runs pipeline**.
4. **Static site is deployed to Apache**.
5. Open:
   ```
   3.88.174.44:80

   ![downimg](https://github.com/user-attachments/assets/b4f383f6-eb81-4a54-bf18-5ee924d7c14c)

   ```

---

## **Fix: Allow Jenkins to Run sudo Without Password**
You need to allow the **Jenkins user** to run `sudo` commands without asking for a password.

### **Step 1: Open the sudoers file**
Run the following command:
```
sudo visudo
```
This opens the sudoers file for editing.

---

### **Step 2: Grant Jenkins User Permission**
Scroll to the bottom and **add this line**:
```
jenkins ALL=(ALL) NOPASSWD: /bin/rm, /bin/cp, /bin/systemctl
```
This allows Jenkins to run `rm`, `cp`, and `systemctl` **without a password**.

---

### **Step 3: Save and Exit**
- Press **Ctrl + X**, then **Y**, and hit **Enter**.

---
---

### **Step 4: Restart Jenkins**
Apply the changes by restarting Jenkins:
```bash
sudo systemctl restart jenkins
```

---

### **Step 6: Run the Pipeline**
- **Push the code** to GitHub.
- **Webhook triggers Jenkins**.
- **Jenkins deploys the website successfully! 🚀**
  ![downimg](https://github.com/user-attachments/assets/fa656fa9-9fd5-46c9-aa5f-ceeeaef0ec7b)


Now, your **static website will deploy automatically** without any password issues.

