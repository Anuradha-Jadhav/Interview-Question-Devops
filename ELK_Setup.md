# **ELK Stack Setup and Configuration**

The **ELK Stack** (Elasticsearch, Logstash, and Kibana) is a powerful logging and monitoring solution that helps collect, analyze, and visualize logs from various sources. It is commonly used for centralized logging, security monitoring, and real-time log analysis.

---

# **1. Overview of ELK Stack Components**
### **1.1 Elasticsearch**
- A distributed search engine that stores and indexes log data.
- Supports full-text search, analytics, and data aggregation.
- Stores structured and unstructured logs for fast querying.

### **1.2 Logstash**
- A data collection and processing pipeline.
- Gathers logs from various sources, processes them, and sends them to Elasticsearch.
- Supports filters, transformations, and data enrichment.

### **1.3 Kibana**
- A web-based visualization tool for Elasticsearch data.
- Provides dashboards, graphs, and real-time log analysis.
- Enables log searching and filtering using Kibana Query Language (KQL).

### **1.4 Beats (Optional)**
- Lightweight data shippers that send logs, metrics, and security events to Logstash or Elasticsearch.
- Examples: Filebeat (log collection), Metricbeat (system metrics), Auditbeat (security logs).

---

# **2. ELK Stack Installation on Linux**
## **2.1 Prerequisites**
- A Linux server (Ubuntu/Debian/RHEL/CentOS).
- Minimum **4GB RAM** and **2 vCPUs** (Recommended 8GB+ for production).
- Java 8 or later (Elasticsearch requires Java).
- Open ports: 
  - **9200** (Elasticsearch)
  - **5601** (Kibana)
  - **5044** (Logstash for Beats)
  - **9300** (Elasticsearch cluster communication)

## **2.2 Install Elasticsearch**
### **Step 1: Download and Install Elasticsearch**
```sh
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-8.x.x-linux-x86_64.tar.gz
tar -xvf elasticsearch-8.x.x-linux-x86_64.tar.gz
mv elasticsearch-8.x.x /usr/local/elasticsearch
```
Or install via APT/YUM:
```sh
# Ubuntu/Debian
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
echo "deb https://artifacts.elastic.co/packages/8.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-8.x.list
sudo apt update && sudo apt install elasticsearch -y

# RHEL/CentOS
sudo rpm --import https://artifacts.elastic.co/GPG-KEY-elasticsearch
echo "[elasticsearch]
name=Elasticsearch repository for 8.x packages
baseurl=https://artifacts.elastic.co/packages/8.x/yum
gpgcheck=1
enabled=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch" | sudo tee /etc/yum.repos.d/elasticsearch.repo
sudo yum install elasticsearch -y
```

### **Step 2: Configure Elasticsearch**
Edit `/etc/elasticsearch/elasticsearch.yml`:
```yaml
cluster.name: my-elk-cluster
node.name: node-1
network.host: 0.0.0.0
http.port: 9200
discovery.type: single-node
```
### **Step 3: Start Elasticsearch**
```sh
sudo systemctl enable elasticsearch
sudo systemctl start elasticsearch
```
Verify:
```sh
curl -X GET "http://localhost:9200"
```

---

## **2.3 Install Logstash**
### **Step 1: Install Logstash**
```sh
sudo apt install logstash -y   # Ubuntu/Debian
sudo yum install logstash -y   # RHEL/CentOS
```

### **Step 2: Configure Logstash**
Create a pipeline configuration file:  
`/etc/logstash/conf.d/logstash.conf`
```yaml
input {
  beats {
    port => 5044
  }
}

filter {
  grok {
    match => { "message" => "%{COMBINEDAPACHELOG}" }
  }
}

output {
  elasticsearch {
    hosts => ["http://localhost:9200"]
    index => "logs-%{+YYYY.MM.dd}"
  }
  stdout { codec => rubydebug }
}
```

### **Step 3: Start Logstash**
```sh
sudo systemctl enable logstash
sudo systemctl start logstash
```

Verify Logstash:
```sh
sudo systemctl status logstash
```

---

## **2.4 Install Kibana**
### **Step 1: Install Kibana**
```sh
sudo apt install kibana -y  # Ubuntu/Debian
sudo yum install kibana -y  # RHEL/CentOS
```

### **Step 2: Configure Kibana**
Edit `/etc/kibana/kibana.yml`:
```yaml
server.port: 5601
server.host: "0.0.0.0"
elasticsearch.hosts: ["http://localhost:9200"]
```

### **Step 3: Start Kibana**
```sh
sudo systemctl enable kibana
sudo systemctl start kibana
```

### **Step 4: Access Kibana**
- Open a web browser and go to:  
  👉 `http://<your-server-ip>:5601`
- Configure **Index Patterns** in Kibana.

---

## **2.5 Install Filebeat (Log Collection)**
### **Step 1: Install Filebeat**
```sh
sudo apt install filebeat -y  # Ubuntu/Debian
sudo yum install filebeat -y  # RHEL/CentOS
```

### **Step 2: Configure Filebeat**
Edit `/etc/filebeat/filebeat.yml`:
```yaml
filebeat.inputs:
  - type: log
    paths:
      - /var/log/*.log

output.logstash:
  hosts: ["localhost:5044"]
```

### **Step 3: Enable Filebeat Modules**
```sh
sudo filebeat modules enable system
sudo filebeat setup
```

### **Step 4: Start Filebeat**
```sh
sudo systemctl enable filebeat
sudo systemctl start filebeat
```

---

# **3. Monitoring Logs in Kibana**
Once Filebeat is running, logs will appear in Kibana:
1. **Go to Kibana → Discover**
2. Select the `logs-*` index.
3. Use filters like:
   ```sh
   message: "error"
   ```

---

# **4. Managing ELK Stack**
## **4.1 Check Running Services**
```sh
systemctl status elasticsearch
systemctl status logstash
systemctl status kibana
systemctl status filebeat
```

## **4.2 Restart ELK Services**
```sh
systemctl restart elasticsearch
systemctl restart logstash
systemctl restart kibana
systemctl restart filebeat
```

## **4.3 Checking Logs for Issues**
```sh
journalctl -u elasticsearch --no-pager | tail -n 50
journalctl -u logstash --no-pager | tail -n 50
journalctl -u kibana --no-pager | tail -n 50
journalctl -u filebeat --no-pager | tail -n 50
```

---

# **5. ELK Stack Security Best Practices**
✅ **Secure Elasticsearch**
- Enable authentication (`xpack.security.enabled: true`).
- Use SSL/TLS (`elasticsearch-certutil` to generate certs).

✅ **Limit Logstash Access**
- Bind Logstash only to required IPs.

✅ **Use a Reverse Proxy**
- Protect Kibana using Nginx with basic auth.

✅ **Monitor System Performance**
- Use Metricbeat to track ELK resource usage.

---

# **6. Summary**
- **Elasticsearch**: Stores and indexes logs.
- **Logstash**: Processes and forwards logs.
- **Kibana**: Visualizes logs.
- **Filebeat**: Ships logs to Logstash.
- **Use Cases**: Centralized logging, security monitoring, real-time analysis.

🚀 **Now your ELK Stack is ready to analyze logs efficiently!**  
