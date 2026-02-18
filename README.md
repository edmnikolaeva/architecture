## Push Notifications Architecture – System Analysis Examples
- This repository presents a high-level architecture diagram of push notification delivery in a mobile application, illustrated as a block diagram
- This diagram helps to understand how push notifications are processed and delivered in a microservices backend, ensuring reliability and scalability

---

### 👉 [Push Notifications Architecture](https://github.com/edmnikolaeva/architecture/blob/main/push_notifications_architecture.txt) 

---

### 🧩 Business Context
- **Problem:** mobile app requires reliable push notification delivery
- **Solution:** designed high-level architecture for push delivery pipeline
- **My role:** system analysis, architecture modeling, API design
- **Result:** defined scalable notification flow supporting high-load delivery

---

### ⚙️ Non-Functional Requirements

- Delivery latency: ≤ 5 seconds
- Throughput: up to 50k notifications/min
- Availability: 99.9%
- Retry policy: exponential backoff
- Monitoring: delivery success rate tracking

### 🚨 Risks

- Push provider rate limits
- Device token invalidation
- Notification delivery delays under peak load

---

**ARCHITECTURE INCLUDES:**
- Microservices
- Load Balancer
- API Gateway
- AuthServer
- Service Registry
- Message Broker
- Notification Service
- Delivery Workers
- Push Provider

**STEPS:**
1. User opens the mobile application  
2. Load Balancer receives the request and forwards it to a free API Gateway instance  
3. Authorization starts via AuthServer (JWT token is generated, pushToken is stored in Redis)  
4. API Gateway queries ServiceRegistry to get the IP of the required microservice  
5. ServiceRegistry returns the address  
6. API Gateway forwards the request to the appropriate microservice  
7. User lands on the main page and can navigate between screens  
8. On navigating to another page, API Gateway and ServiceRegistry check the user token again  
9. For catalog searches, ElasticSearch is used for faster access  
10. Perhaps user had items in the cart that were not paid for  
11. Cart create an "order not completed" event and forward it to Kafka  
12. Notification Service, subscribed to the topic, "hears" this, retrieves the message, gets pushToken from Redis and forwards the message to Delivery Workers  
13. Delivery Workers make an HTTP request to the external Push Provider  
14. If delivery fails after several retries, the message goes to DLQ  
15. If successful, Push Provider delivers the notification to the user’s device

---

### [Sorry, Mario, but our C4 Model is in another repository](https://github.com/edmnikolaeva/C4)
