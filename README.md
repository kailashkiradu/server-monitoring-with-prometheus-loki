# server-monitoring-with-prometheus-loki

A complete observability solution using Node.js, Prometheus, Loki, and Grafana. This project demonstrates how to collect logs and performance metrics from a custom Express server, send logs to Loki, expose metrics for Prometheus, and visualize everything in Grafana dashboards.

---

## Table of Contents

- [Overview](#overview)  
- [Project Components](#project-components)  
- [Prerequisites](#prerequisites)  
- [Setup and Installation](#setup-and-installation)  
- [Running the Project](#running-the-project)  
- [How It Works](#how-it-works)  
- [Using Grafana Dashboards](#using-grafana-dashboards)  
- [Deploying to a Real Server](#deploying-to-a-real-server)  
- [Troubleshooting](#troubleshooting)  
- [Future Improvements](#future-improvements)  
- [License](#license)

---

## Overview

This project creates a simulated Node.js server that generates logs and performance metrics. Logs are shipped to Loki, while metrics are exposed to Prometheus. Grafana connects to both Loki and Prometheus to provide a unified dashboard for monitoring server health, error logs, and request performance.

![Screenshot 2025-06-05 121429](https://github.com/user-attachments/assets/3e494fd5-0454-4b86-a2a7-e53a010c206c)


---

## Project Components

| Component          | Role                                                                                  |
|--------------------|---------------------------------------------------------------------------------------|
| `index.js`         | Express server that generates logs and exposes metrics                                |
| `util.js`          | Simulates slow requests and occasional errors                                        |
| Prometheus         | Collects metrics exposed at `/metrics` endpoint                                       |
| Loki               | Aggregates logs sent via Winston Loki Transport                                      |
| Grafana            | Visualizes logs and metrics in customizable dashboards                               |
| Docker Compose     | Runs Prometheus, Loki, and Grafana containers for easy setup                         |

---

## Prerequisites

- Node.js (v14+ recommended)
- npm or yarn
- Docker and Docker Compose installed
- Basic knowledge of terminal/command line

---

## Setup and Installation

1. **Clone the repository**

   git clone <repository-url>
   cd <repository-folder>

2. **Install Node.js dependencies**

   npm install

3. **Configure Prometheus**

   Edit `prometheus-config.yml` to set your server IP or hostname under `targets`.

4. **Run Docker containers for Loki, Prometheus, and Grafana**

   Ensure `docker-compose.yml` is in the project root, then run:

   docker-compose up -d

   This will start the following services:

   * Prometheus on port `9090`
   * Loki on port `3100`
   * Grafana on port `3000`

---

## Running the Project

1. **Start your Node.js server**

   node index.js

   The server will listen on port `8000` by default.

2. **Verify endpoints**

   * Visit `http://localhost:8000/` to see a simple response
   * Visit `http://localhost:8000/slow` to simulate a slow request
   * Visit `http://localhost:8000/metrics` to view raw Prometheus metrics

3. **Access Grafana dashboard**

   Open `http://localhost:3000` in your browser.
   Default login:

   * User: `admin`
   * Password: `admin`

4. **Add data sources in Grafana**

   * Loki URL: `http://localhost:3100`
   * Prometheus URL: `http://localhost:9090`

5. **Import dashboards**

   Use pre-made dashboards or create your own to visualize logs and metrics.

---

## How It Works

* **Logging**

  The Express app uses Winston with the Loki transport to send logs to the Loki server. Logs include info, error, and simulated critical levels.

* **Metrics**

![Screenshot 2025-06-05 121334](https://github.com/user-attachments/assets/fc9ae966-0e7e-45d3-aa8b-627d25db0df6)


  Prom-client collects default system metrics and custom metrics such as request count and response times. These are exposed on `/metrics` endpoint for Prometheus scraping.

* **Prometheus**

![Screenshot 2025-06-05 121419](https://github.com/user-attachments/assets/7856b9dc-d5b5-4531-a25e-7bbdb401ae34)


  Scrapes the `/metrics` endpoint periodically to collect and store metrics.

* **Docker**

![Screenshot 2025-06-05 121352](https://github.com/user-attachments/assets/e2a960e6-bb64-4efe-8dbf-31447c590df7)


* **Loki**

![Screenshot 2025-06-05 122900](https://github.com/user-attachments/assets/cc1b1dd3-c18c-4a6e-ad29-169783b1465d)


  Aggregates log data for querying and visualization.

* **Grafana**



  Connects to Prometheus and Loki to provide visual monitoring, alerting, and log analysis.

---

## Using Grafana Dashboards

* Monitor request rates, response times, and error rates.
* Filter logs by severity (info, error, critical).
* Set alerts for critical log messages or metric thresholds.
* Correlate logs and metrics to quickly diagnose issues.

![Screenshot 2025-06-05 122756](https://github.com/user-attachments/assets/3fbf1c83-6133-4a98-bfa1-fdfd2bf8c3d1)


---

## Deploying to a Real Server

1. **Copy your Node.js app to the server**

   Use `scp` or `rsync` to transfer files.

2. **Install Node.js and dependencies on the server**

3. **Install and configure Loki, Prometheus, and Grafana**

   Use Docker Compose or install natively.

4. **Update configuration files**

   Replace localhost IPs with server IPs or domain names.

5. **Open necessary firewall ports**

   * 8000 (Node.js app)
   * 9090 (Prometheus)
   * 3100 (Loki)
   * 3000 (Grafana)

6. **Run services and verify**

---

## Troubleshooting

* **Logs not appearing in Grafana:**
  Check Loki URL in Grafana data sources and network connectivity.

* **Metrics missing:**
  Verify Prometheus scrape config targets the correct IP and port.

* **Port conflicts:**
  Make sure no other services use the required ports.

* **Docker issues:**
  Check container logs using `docker logs <container-name>`.

---

## Future Improvements

* Add alerting rules in Prometheus/Grafana for critical logs
* Support for multiple environments (dev, staging, prod)
* Persist Loki and Prometheus data volumes for durability
* Add authentication for Grafana dashboards

---

## License

This project is open-source and available under the [MIT License](LICENSE).

---
