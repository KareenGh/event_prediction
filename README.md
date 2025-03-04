# 🚀 Event Prediction System

## 📌 Overview
This system processes logs using Kafka and multiple microservices, enabling event prediction based on log data.

---

## 📌 Prerequisites
Ensure you have the following installed before running the project:

- **Docker & Docker Compose**
  ```bash
  docker --version
  docker-compose --version
  ```

- **Git (to clone the repository)**
  ```bash
  git --version
  ```

---

## 📌 How to Set Up & Run the Project

### **1️⃣ Clone the Repository**
```bash
git clone <repository-url>
cd event_prediction_dummy
```

### **2️⃣ Start the System**
```bash
docker-compose up -d --build
```
✅ This will:
- Build and start all services.
- Automatically create Kafka topics (if missing).

### **3️⃣ Verify That Everything is Running**
Check if all services are running:
```bash
docker ps
```
✅ **Expected output:** All services should be **Up**.

Check Kafka topics:
```bash
docker exec -it kafka kafka-topics.sh --bootstrap-server kafka:9092 --list
```
✅ **Expected topics:**  
```
lfr-topic
ept-topic
mm-topic
esm-topic
llm-topic
col-topic
rm-topic
```

### **4️⃣ View Logs for a Specific Service**
```bash
docker logs event_prediction_lfr --follow
```
✅ **Expected output:**
```
Connected to Kafka!
Sent to lfr-topic: Log from LFR container
```

### **5️⃣ Open Kafka UI (Optional)**
- **Visit** `http://localhost:8080`
- Click on a topic and view real-time messages.

---

## 📌 Debugging & Troubleshooting

### **🛑 Issue: Services Keep Restarting**
```bash
docker ps
```
❌ **If services show `Restarting (1)` continuously:**
1. **Check logs of a failing service:**
   ```bash
   docker logs event_prediction_lfr --follow
   ```
2. **If `NoBrokersAvailable` error appears:**
   - Restart Kafka & Zookeeper:
     ```bash
     docker-compose restart kafka zookeeper
     ```
   - Wait 5 seconds and restart other services:
     ```bash
     docker-compose restart event_prediction_lfr event_prediction_ept
     ```

### **🛑 Issue: Kafka Topics Not Found**
```bash
docker exec -it kafka kafka-topics.sh --bootstrap-server kafka:9092 --list
```
❌ **If topics are missing, manually create them:**
```bash
docker exec -it kafka kafka-topics.sh --bootstrap-server kafka:9092 --create --topic lfr-topic --partitions 1 --replication-factor 1
```
*(Repeat for other missing topics.)*

### **🛑 Issue: Logs Not Showing**
1. **Check if a service is connected to Kafka:**
   ```bash
   docker logs event_prediction_lfr --follow
   ```
2. **Ensure Kafka UI is running:**  
   ```bash
   docker logs kafka-ui --follow
   ```

### **🛑 Issue: Kafka UI Not Accessible**
1. **Check if Kafka UI is running:**
   ```bash
   docker ps | grep kafka-ui
   ```
2. **Restart Kafka UI:**
   ```bash
   docker-compose restart kafka-ui
   ```

### **🛑 Reset Everything (Last Resort)**
If nothing works, reset the system completely:
```bash
docker-compose down --volumes
docker-compose up -d --build
```

---

## 📌 Next Steps
- Integrate real logic into the microservices.
- Test the system with real log data.
- Optimize Kafka consumer-producer efficiency.

🚀 **Now, everything is set up! If you run into any issues, check the troubleshooting steps.** 🎯
