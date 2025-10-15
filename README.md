# Fusionpact DevOps Challenge – Level 1: Cloud Deployment

## 🚀 Objective

Deploy the full stack application on an AWS Ubuntu EC2 instance using Docker and Docker Compose with MySQL persistent storage.  
- **Frontend:** Accessible on port `80`  
- **Backend:** Accessible on port `8000`

---

## 📁 Repository Structure

```
.
├── README.md
├── backend
│   ├── Dockerfile
│   ├── requirements.txt
│   └── app/
├── frontend
│   ├── Dockerfile
│   └── Devops_Intern.html
├── docker-compose.yml
└── SOP Level1.pdf
```

---

## 🛠️ Prerequisites

- Docker installed on the server
- Docker Compose installed
- AWS EC2 Ubuntu instance with security groups allowing:
  - HTTP (`80`)
  - Backend (`8000`)
  - MySQL (`3306`, optional for direct access)

---

## 📝 Deployment Steps

### 1. Clone the Repository

```bash
git clone <YOUR-FORKED-REPO-URL>
cd fusionpact-devops-challenge
```

### 2. Build and Start Containers

```bash
sudo docker-compose down --volumes --remove-orphans
sudo docker-compose up -d --build
```

### 3. Verify Running Containers

```bash
sudo docker ps
```
You should see `frontend`, `backend`, and `mysql-db` containers running.

---

## 🌐 Accessing the Application

- **Frontend:** [http://<EC2-PUBLIC-IP>/](http://<EC2-PUBLIC-IP>/)
- **Backend:** [http://<EC2-PUBLIC-IP>:8000](http://<EC2-PUBLIC-IP>:8000)

---

## 🗄️ MySQL Persistence Verification

1. **Connect to MySQL container:**
    ```bash
    sudo docker exec -it mysql-db mysql -u fusionuser -p
    # Password: fusionpass
    ```

2. **Verify database and tables:**
    ```sql
    USE fusionpact;
    SHOW TABLES;
    ```

3. **Insert test data to verify persistence:**
    ```sql
    CREATE TABLE test_table (id INT AUTO_INCREMENT PRIMARY KEY, name VARCHAR(50));
    INSERT INTO test_table (name) VALUES ('Level1 Test');
    SELECT * FROM test_table;
    ```

4. **Stop and restart containers, then verify data still exists.**

> **Note:**  
> The MySQL database uses a Docker volume (`mysql_data`) to persist data across container restarts.

---

## 🐳 Docker Compose Services

| Service   | Ports      | Description                      |
|-----------|------------|----------------------------------|
| frontend  | 80:80      | Nginx serving HTML frontend      |
| backend   | 8000:8000  | FastAPI backend service          |
| db        | 3306:3306  | MySQL database with persistence  |

---

## 📸 Screenshots (for SOP)

- Frontend page in browser
- Backend API endpoint
- MySQL showing persisted data

---

## 👤 Author

Dhruv Shah

---
