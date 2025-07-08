## 🎯 **Learning Objectives**

After this unit, you will be able to:

- Identify the **3 layers** used to evaluate integration patterns.
    
- Describe the **2 types of timing** (synchronous vs. asynchronous) and their importance.
    

---

## 🧱 **The Four Dimensions of the Layer Approach**

When evaluating Salesforce integration patterns, architects assess **four key dimensions**:

|**Dimension**|**Purpose**|
|---|---|
|**Layers**|Logical grouping of tasks: UI, Business Process, and Data layers|
|**Volume**|Amount of data transferred and transformed|
|**Timing**|Synchronous (real-time) vs. Asynchronous (batch/later) communication|
|**Direction**|The source and target of the integration (Salesforce → System or vice versa)|

---

## 🧩 **Three Layers of Application Architecture**

|**Layer**|**Role**|**Examples**|
|---|---|---|
|**User Interface (UI)**|Handles user interaction and UI mashups|Salesforce Canvas, Lightning Out|
|**Business Process**|Manages logic, validation, and workflow|Platform Events, Flows, Outbound Messaging, Mulesoft|
|**Data Layer**|Handles data access, replication, and transformation|REST/SOAP, Heroku Connect, Bulk API, Salesforce Connect|

### 🔄 Mapping Patterns to Layers

|**Pattern**|**Layer**|
|---|---|
|Remote Process Invocation – Request & Reply|Business Process|
|Remote Process Invocation – Fire & Forget|Business Process|
|Remote Call-In|Business Process|
|Batch Data Synchronization|Data|
|Data Virtualization|Data|
|High-Frequency Data Replication|Data & UI|
|Publish/Subscribe|Data & Business Process|

---

## 📊 **Volume**

- Important due to **Salesforce Governor Limits** (like DML limits, API call quotas).
    
- Choose patterns that match your volume needs:
    
    - Small Volume → _Request & Reply_
        
    - Large Volume → _Batch Data Synchronization, Heroku Connect_
        

---

## ⏱️ **Timing**

### Two Types:

- **Synchronous**: Waits for a response (like a phone call)
    
- **Asynchronous**: Doesn’t wait; response handled separately (like an email)
    

|**Pattern**|**Timing**|
|---|---|
|Remote Process Invocation – Request & Reply|Synchronous|
|Remote Process Invocation – Fire & Forget|Asynchronous|
|Data Virtualization|Synchronous|
|Remote Call-In|Either (depends on setup)|
|Batch Data Synchronization|Asynchronous|
|High-Frequency Data Replication|Either (based on setup)|
|Publish/Subscribe|Asynchronous|

---

## 🔁 **Direction**

|**Direction**|**Meaning**|
|---|---|
|**Salesforce → System**|Salesforce initiates the interaction|
|**System → Salesforce**|External system initiates the interaction|

### Pattern Direction Mapping:

|**Pattern**|**Direction**|
|---|---|
|Remote Process Invocation – Request & Reply|Salesforce → System|
|Fire & Forget|Salesforce → System|
|Publish/Subscribe|Salesforce → System|
|Data Virtualization|Salesforce → System|
|High-Frequency Data Replication|Salesforce → System|
|Remote Call-In|System → Salesforce|
|Batch Data Synchronization|Bidirectional (primarily System → Salesforce)|

---

## 🔐 **Security**

- Secure data with **Shield Platform Encryption** for Salesforce → System integrations.
    
- Incorporate **SSO, SAML, PII protection**.
    

---

## ⚠️ **Error Handling & Recovery**

- **Error Handling**: Captures and manages errors (e.g., invalid data, failed API call).
    
- **Recovery**: Changes are only committed after success; retries may be required.
    

---

## 🔄 **Coupling**

- Prefer **loosely coupled** systems for flexibility and lower dependency.
    
- Messaging-based async solutions are usually **loosely coupled**.
    
- **Tightly coupled** systems require more coordination and can break more easily.
    

---
