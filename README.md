# Cloud Native Anomaly Detection Pipeline

A real-time, container-based pipeline that collects logs and metrics from cloud services, streams them through Apache Kafka, and uses machine learning (Isolation Forest) to detect unusual behaviour automatically. Detected anomalies are stored, sent as alerts, and shown on a Grafana dashboard.

> **Status:** Work in progress (about 50% complete). See [Project Status](#project-status).

> Mini project for B.Tech CSE (AIML), D. Y. Patil Agriculture & Technical University, Talsande (2025-26).

---

## Table of Contents

- [Why this project](#why-this-project)
- [Features](#features)
- [How it works](#how-it-works)
- [Tech stack](#tech-stack)
- [Project structure](#project-structure)
- [Getting started](#getting-started)
- [Project status](#project-status)
- [Future scope](#future-scope)
- [Team](#team)
- [References](#references)

---

## Why this project

Modern applications run as many small services (microservices) on cloud platforms like AWS, Azure and GCP. These services produce huge amounts of logs and metrics every second. Watching all of it by hand is not possible, and old monitoring methods have problems:

- Fixed thresholds (for example, "alert if CPU > 80%") miss new kinds of problems.
- They create many false alarms, because normal usage changes during the day.
- Problems are found late, often after users are already affected.
- Many ML solutions run offline and do not scale on the cloud.

This project builds one complete pipeline that **collects data, detects anomalies with machine learning, and sends alerts**, and runs on Docker and Kubernetes so it can scale.

## Features

- Real-time collection of logs and metrics from microservices
- Streaming with Apache Kafka
- Data cleaning, normalization and feature extraction (CPU usage, memory, latency, error rate, request count)
- Anomaly detection with Isolation Forest (Autoencoder planned as a second model)
- Storage of raw data and detected anomalies
- Alerts by email or Slack
- Live graphs and anomalies on a Grafana dashboard
- Containerized with Docker and managed by Kubernetes
- Modular design, so each part can be changed or scaled on its own

## How it works

```
Cloud services (logs, metrics)
        |
        v
Collectors (Prometheus exporters, Fluentd)
        |
        v
Apache Kafka  --->  Preprocessing and feature extraction (Python)
                              |
                              v
                    Anomaly detection model
                    (Isolation Forest)
                       |              |
                       v              v
                  Database       Alert manager
            (InfluxDB / Elasticsearch   (Email / Slack)
                  / MySQL)
                       |
                       v
               Grafana dashboard
```

1. **Data generation:** microservices produce logs and metrics.
2. **Ingestion:** collectors send the data to Kafka topics.
3. **Processing:** data is cleaned, normalized and turned into features.
4. **Detection:** the model gives every record an anomaly score. Records above the threshold are marked as anomalies.
5. **Storage:** raw data and results are saved.
6. **Alerting and visualization:** alerts go out by email or Slack, and Grafana shows trends and anomalies.

### About the model

Isolation Forest builds many random trees. Records that are very different from the rest are separated in only a few splits, so they get a high anomaly score. It is fast, needs little memory and works without labelled data, which suits monitoring data.

## Tech stack

| Area | Tools |
|---|---|
| Language | Python |
| Machine learning | Scikit-learn (Isolation Forest), TensorFlow/Keras (Autoencoder), Pandas, NumPy |
| Streaming | Apache Kafka |
| Data collection | Prometheus exporters, Fluentd |
| Containers | Docker, Kubernetes |
| Storage | InfluxDB / Elasticsearch / MySQL |
| Visualization | Grafana |
| Dev tools | VS Code, Git/GitHub, Jupyter Notebook |

## Project structure

> Update this section to match the folders in your repository.

```
.
├── data/              # dataset and sample data
├── notebooks/         # Jupyter notebooks (exploration, training)
├── src/
│   ├── collector/     # data collection
│   ├── processing/    # cleaning and feature extraction
│   └── model/         # training and anomaly detection
├── docker/            # Dockerfiles and compose files
├── k8s/               # Kubernetes manifests
├── dashboards/        # Grafana dashboards
├── docs/              # report, diagrams, screenshots
└── README.md
```

## Getting started

> Replace the commands below with the ones that work in your repository.

### Prerequisites

- Python 3.9 or later
- Docker and Docker Compose
- (Optional) A Kubernetes cluster such as Minikube or kind

### Run locally

```bash
# 1. Clone the repository
git clone https://github.com/<your-username>/<repo-name>.git
cd <repo-name>

# 2. Install Python dependencies
pip install -r requirements.txt

# 3. Start Kafka and the other services
docker compose up -d

# 4. Train the model
python src/model/train.py

# 5. Start the detection service
python src/model/detect.py
```

## Project status

| Task | Status |
|---|---|
| Dataset collection and preprocessing | Done |
| Isolation Forest model trained and tested | Done |
| Docker containers for the main components | Done |
| Kafka data pipeline | Done |
| Kubernetes deployment | In progress |
| Grafana dashboard | In progress |
| Alert integration (email / Slack) | In progress |
| Performance testing and tuning | Planned |
| Documentation | Planned |

## Future scope

- Deep learning models (LSTM / Transformer) for complex time-series patterns
- Auto-remediation, such as restarting or scaling a service when an anomaly appears
- Root cause analysis to find which service caused the problem
- Multi-cloud support (AWS, Azure, GCP)
- Online / continuous learning as the data changes
- Security threat detection

## Team

| Name | PRN |
|---|---|
| Darshan Chidanand Bagale | 2024011041003 |
| Rahul Virabhadra Teli | 2024011041114 |
| Satyam Anil Phalke | 2024011041093 |
| Shivsai Nilesh Jagadale | 2024011041036 |
| Somnath Satish Khot | 2024011041048 |

**Guide:** Mr. R.S. Todkar
**Department:** Computer Science & Engineering (AIML), School of Engineering & Technology, D. Y. Patil Agriculture & Technical University, Talsande

## References

- Chandola, V., Banerjee, A., Kumar, V., "Anomaly Detection: A Survey," ACM Computing Surveys, 2009.
- Liu, F. T., Ting, K. M., Zhou, Z., "Isolation Forest," IEEE International Conference on Data Mining, 2008.
- Official documentation: Apache Kafka, Kubernetes, Docker, Prometheus, Grafana, Scikit-learn, TensorFlow

## License

Add a license of your choice (for example, MIT) or remove this section.
