# Security Monitoring Pipeline

This project demonstrates a security monitoring pipeline using the **ELK Stack** (Elasticsearch, Logstash, Kibana) with **Filebeat** to collect and ship logs from Docker containers. It is designed to showcase centralized logging and observability for security analytics.

---

## Table of Contents

- [Project Overview](#project-overview)
- [Components](#components)
- [Architecture](#architecture)
- [Setup Instructions](#setup-instructions)
- [Validation](#validation)
- [Troubleshooting](#troubleshooting)
- [License](#license)

---

## Project Overview

The pipeline collects logs from running Docker containers, ships them via **Filebeat**, stores them in **Elasticsearch**, and visualizes them in **Kibana**. This setup is commonly used for monitoring containerized applications and maintaining security observability.

---

## Components

- **Elasticsearch**: Stores and indexes log data.  
- **Kibana**: Visualization layer for dashboards and log analysis.  
- **Filebeat**: Lightweight shipper to collect container logs and send to Elasticsearch.  
- **Docker Network**: Connects all services for seamless communication.

---

## Architecture


yaml file

- Filebeat runs as a container, monitoring all Docker container logs.
- Elasticsearch receives logs and indexes them.
- Kibana connects to Elasticsearch to visualize and explore logs.

---

## Setup Instructions

### 1. Create Docker Network
```bash
docker network create elk-network

docker run -d --name elasticsearch --net elk-network -p 9200:9200 -e "discovery.type=single-node" docker.elastic.co/elasticsearch/elasticsearch:8.11.0

docker run -d --name kibana --net elk-network -p 5601:5601 docker.elastic.co/kibana/kibana:8.11.0

docker run -d --name filebeat --net elk-network -v /var/lib/docker/containers:/var/lib/docker/containers:ro -v $(pwd)/filebeat.yml:/usr/share/filebeat/filebeat.yml docker.elastic.co/beats/filebeat:8.11.0

filebeat.inputs:
  - type: log
    enabled: true
    paths:
      - /var/lib/docker/containers/*/*.log
    json.keys_under_root: true
    json.add_error_key: true

output.elasticsearch:
  hosts: ["http://elasticsearch:9200"]
  username: "elastic"
  password: "changeme"
  ssl.enabled: false

setup.kibana:
  host: "http://kibana:5601"


---

If you want, I can also **write a shorter, more beginner-friendly version** that’s visually cleaner and easier to follow for GitHub viewers.  

Do you want me to do that?
