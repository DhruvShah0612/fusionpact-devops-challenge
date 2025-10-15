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

## Level 2 – Monitoring & Observability

### 🚦 Objective

Implement complete observability for the deployed application by monitoring both application metrics and infrastructure metrics using Prometheus, Node Exporter, and Grafana.

---

### 🧩 Components Used

| Component      | Purpose                                                        |
| -------------- | -------------------------------------------------------------- |
| Prometheus     | Collects metrics from the backend application and Node Exporter|
| Grafana        | Visualizes metrics in dashboards for real-time monitoring      |
| Node Exporter  | Exposes host-level metrics (CPU, memory, disk) for Prometheus  |
| Docker Compose | Deploys all services in isolated containers                    |

---

### ⚙️ Setup Instructions

#### 1. Docker Compose

Use the following [`docker-compose.yml`](docker-compose.yml) for Level 2:

```yaml
version: '3.9'

services:
  x-prometheus:
    image: prom/prometheus:latest
    container_name: prometheus
    ports:
      - "9090:9090"
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml
    networks:
      - fusionnet

  x-grafana:
    image: grafana/grafana:latest
    container_name: grafana
    ports:
      - "3000:3000"
    environment:
      - GF_SECURITY_ADMIN_USER=admin
      - GF_SECURITY_ADMIN_PASSWORD=admin
    networks:
      - fusionnet
    depends_on:
      - x-prometheus

  x-node-exporter:
    image: prom/node-exporter:latest
    container_name: node-exporter
    ports:
      - "9100:9100"
    networks:
      - fusionnet
    restart: always

networks:
  fusionnet:
    driver: bridge
```

---

#### 2. Prometheus Configuration ([`prometheus.yml`](prometheus.yml))

```yaml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'backend'
    static_configs:
      - targets: ['backend:8080']  # replace with actual backend host and port

  - job_name: 'node-exporter'
    static_configs:
      - targets: ['x-node-exporter:9100']
```

Prometheus scrapes metrics from the backend application and Node Exporter every 15 seconds.

---

#### 3. Start Services

```sh
docker-compose up -d
```

Verify containers are running:

```sh
docker ps
```

---

#### 4. Access Grafana

- URL: `http://<EC2-PUBLIC-IP>:3000`
- Username: `admin`
- Password: `admin`

Add Prometheus as a data source:

- URL: `http://x-prometheus:9090`
- Click **Save & Test** → should show "Data source is working" ✅

---

---
#### 5. Create Dashboards

**Application Metrics Panels:**

| Metric                      | Label Filter            | Example Value           | Description                                           |
|-----------------------------|------------------------|------------------------|-------------------------------------------------------|
| `http_requests_total`       | handler, method, status| /api/login, GET, 200   | Total HTTP requests per endpoint                      |
| `http_request_duration_seconds` | handler, le, method | /api/register, le="0.5"| Request latency per endpoint (used for percentiles)   |

**Infrastructure Metrics Panels (Node Exporter):**

| Metric                                 | Label Filter        | Example Value | Description                |
|-----------------------------------------|--------------------|--------------|----------------------------|
| `node_cpu_seconds_total`                | mode, instance     | user, idle   | CPU usage per mode         |
| `node_filesystem_avail_bytes`           | mountpoint, instance| /, /home    | Disk space available       |
| `node_network_receive_bytes_total`      | interface, instance| eth0         | Network bytes received     |
| `node_network_transmit_bytes_total`     | interface, instance| eth0         | Network bytes transmitted  |

---
