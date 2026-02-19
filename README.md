# Push Notifications Architecture – System Analysis Examples
- Event-driven push notification pipeline for mobile grocery app 
- The solution demonstrates how push notifications can be processed and delivered in a microservices backend
- Focus: scalability, async delivery, fault tolerance

---

### Main artifact 👉 [Push Notifications Architecture](https://github.com/edmnikolaeva/architecture/blob/main/push_notifications_architecture.txt) 

---

### 🧩 Business Context
- **Domain:** online grocery e-commerce
- **Goal:** recover abandoned carts, notify on order changes, run targeted promotions  
- **Initiative:** implement a reliable push notification system in the mobile app to boost user engagement 

---

**Business Value**  
- Increased recovery of abandoned carts → higher conversion rate  
- Faster reaction to order issues → reduced cancellations & support load  
- More effective marketing → improved LTV and retention  
- Better user experience through timely & relevant communication

---

**Key triggers**  
- Abandoned cart reminders (order sitting too long without checkout)  
- Order status changes (cancellations, delivery delays, etc.)  
- Personalized promotions & marketing campaigns  
- Other event-driven triggers (restocks, favorite product discounts, etc.)
  
---


**Solution**
- Event-driven architecture for better scalability  
- Asynchronous delivery to protect core services  
- DLQ ensures failed messages are not lost  
- Redis is used for fast token access  
- Kafka enables loose coupling between services

---

### ⚙️ Non-Functional Requirements

> Values below reflect design assumptions based on typical industry targets

- Delivery latency: ≤ 5 seconds
- Throughput: up to 50k notifications/min
- Availability: 99.9%
- Retry policy: exponential backoff
- Monitoring: delivery success rate tracking

---

### 🚨 Risks

- Over-notification may reduce user engagement
- Provider rate limits → backoff + priority queues  
- Token invalidation → periodic cleanup + 404/410 handling  
- Peak load → horizontal worker scaling

---

## 👩‍💼 My Role
 
- Problem analysis & high-level design  
- Flow modeling & component identification  
- Non-functional & risk assessment
  
---

**ARCHITECTURE INCLUDES**
- Microservices
- Load Balancer
- API Gateway
- AuthServer
- Service Registry
- Message Broker
- Notification Service
- Delivery Workers
- Push Provider

**Core Workflow:**
1. User authenticates → push token saved to Redis  
2. Business event occurs (e.g. abandoned cart) → Kafka  
3. Notification Service consumes event → fetches token  
4. Enqueues payload to Delivery Workers  
5. Workers send to Push Provider
6. Success → push delivered to the user device
7. Failure → retry → DLQ after max attempts

---

## 🔗 Related Artifact

> Sorry, Mario, but our C4 Model is in another repository 

👉 [C4 Model Repository](https://github.com/edmnikolaeva/C4)
