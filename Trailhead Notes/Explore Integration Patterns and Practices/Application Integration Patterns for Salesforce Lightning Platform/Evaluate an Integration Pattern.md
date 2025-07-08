## 🎯 **Learning Objectives**

- Identify the best solution for implementing this pattern.
    
- Understand the benefits of using this pattern.
    

---

## 📘 **What is Remote Process Invocation — Request and Reply?**

Also called **Request and Response**, this pattern is used when:

- Salesforce sends a **request** to an **external system**, and
    
- Salesforce **waits for a reply** before proceeding.
    

### ✅ **Key Characteristics:**

|Attribute|Value|
|---|---|
|**Layer**|Business Logic|
|**Timing**|Synchronous|
|**Direction**|Salesforce → System|
|**Volume**|Ideal for **small**, real-time messages|

---

## 🧪 **Use Case Example**

Imagine Salesforce manages leads and opportunities, but your orders are handled in an **external system**. You want to connect both so that Salesforce can show **live order details** from the external system.

---

## ⚙️ **How the Pattern Works (Step-by-Step)**

1. **User clicks a button** in a Lightning Web Component (LWC).
    
2. An **Apex controller** is called using an HTTP POST request.
    
3. Apex makes a **call to an external system (web service)**.
    
4. External system **responds synchronously**.
    
5. Apex processes the response and **updates the Salesforce record**.
    
6. The updated results are shown on the Lightning page.
    

---

## 🛠️ **Solution Options:**

|**Solution**|**Recommendation**|
|---|---|
|**LWC + Apex / Visualforce + Apex**|✅ Best option. Event-driven and real-time; matches user interactions.|
|**Apex Triggers**|❌ Not ideal. Asynchronous execution; limited control.|
|**Apex Batch Classes**|❌ Not ideal. Batch calls are for large-scale processing, not real-time.|
|**Flow Builder with External Services**|✅ Also a good solution for real-time, declarative integrations.|

---

## 🔁 **Idempotency Considerations**

To **avoid processing duplicates**, the external system (receiver) must be idempotent.

### Tips:

- Add a **unique message ID** to requests.
    
- The receiver should **check for duplicate records**.
    
- Helps ensure **data integrity** and prevents **wasted resources**.
    

---
