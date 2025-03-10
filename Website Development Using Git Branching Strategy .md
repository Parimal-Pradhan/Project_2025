# **📌 Project: Website Development Using Git Branching Strategy**  
This project follows the **Git Flow** strategy using branches like:  
✅ `master` – Production (Live Website)  
✅ `develop` – Ongoing Development  
✅ `feature-*` – Feature Branches  
✅ `release-*` – Final Testing Before Deployment  
✅ `hotfix-*` – Urgent Bug Fixes  

---

## **📌 Step 1: Initialize the Git Repository**
```bash
mkdir git-branching-project  
cd git-branching-project  
git init  
```
👉 **This creates a new Git repository.**  

---

## **📌 Step 2: Create the Master Branch (Production)**
```bash
git checkout -b master  
echo "<h1>Welcome to My Website</h1>" > index.html  
git add index.html  
git commit -m "Initial commit - Added homepage"  
```
👉 The **master** branch represents the **live website**.

---

## **📌 Step 3: Create a Develop Branch (Ongoing Work)**
```bash
git checkout -b develop  
```
👉 The `develop` branch is used for **ongoing development work**.

---

## **📌 Step 4: Create a Feature Branch (New Feature Development)**
```bash
git checkout -b feature-login-page  
echo "<h2>Login Page</h2>" > login.html  
git add login.html  
git commit -m "Added login page feature"  
```
👉 The `feature-login-page` branch is used to develop a new **Login Page**.

---

## **📌 Step 5: Merge Feature Branch into Develop**
```bash
git checkout develop  
git merge feature-login-page  
git branch -d feature-login-page  # Delete feature branch after merge  
```
👉 Once development is done, it is **merged into `develop`** for testing.

---

## **📌 Step 6: Create a Release Branch (Final Testing Before Deployment)**
```bash
git checkout -b release-v1.0  
echo "<p>Final testing before release</p>" >> login.html  
git commit -am "Final changes before release"  
```
👉 The `release-v1.0` branch is created to **test** the new feature.

---

## **📌 Step 7: Merge Release into Master (Deploy to Live Website)**
```bash
git checkout master  
git merge release-v1.0  
git branch -d release-v1.0  
```
👉 The feature is now **live on production** (master branch).

---

## **📌 Step 8: Hotfix Branch (Fix Urgent Bug in Production)**
```bash
git checkout -b hotfix-login-error  
echo "<p>Fixed login page error</p>" >> login.html  
git commit -am "Fixed critical login bug"  
```
👉 Hotfix branches are created for **urgent production issues**.

---

## **📌 Step 9: Merge Hotfix into Master and Develop**
```bash
git checkout master  
git merge hotfix-login-error  

git checkout develop  
git merge hotfix-login-error  

git branch -d hotfix-login-error  
```
👉 The **fix is applied** to both **master** (live) and **develop** (ongoing work).

---

# **📌 Final Folder Structure**
```
git-branching-project/
│── index.html  (Homepage)
│── login.html  (Login Page)
│── .git/       (Git Repository)
```

# **📌 Final Branch Structure**
```
master (Live Website)
  ├── develop (Ongoing Work)
  │     ├── feature-login-page (New Feature)
  │     ├── release-v1.0 (Final Testing)
  │     ├── hotfix-login-error (Urgent Fix)
```

---

# **📌 Real-World Scenario**
💡 Suppose your team is building a website:  
1️⃣ A developer starts working on a **Login Page** (`feature-login-page` branch).  
2️⃣ After completion, it is merged into the **Develop** branch for testing.  
3️⃣ Before launching, a **Release** branch is created (`release-v1.0`).  
4️⃣ Once approved, it is merged into **Master** (Live website).  
5️⃣ If a bug is found in production, a **Hotfix** branch (`hotfix-login-error`) is created and merged into both **Master** and **Develop**.

---

## **✅ Why Use This Strategy?**
✅ Keeps production stable.  
✅ Allows multiple developers to work on different features.  
✅ Quick bug fixes without disturbing ongoing work.  

