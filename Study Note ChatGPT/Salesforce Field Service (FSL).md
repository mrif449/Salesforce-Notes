## 1. Foundations of Field Service

### 🔹 What is FSL?

Field Service Lightning is Salesforce’s tool for **managing on-site jobs** (repairs, installations, inspections, deliveries, etc.). It extends **Service Cloud** to handle:

- Work orders
    
- Scheduling & dispatch
    
- Mobile field technicians
    
- Inventory management
    

---

### 🔹 Core Data Model (Objects)

1. **Work Order (WO)**
    
    - Represents a service request (job to be done).
        
    - Can include multiple service appointments.
        
    - Linked to Accounts, Cases, Assets.
        
    - Example: "Fix Air Conditioner for Account X."
        
2. **Work Order Line Item (WOLI)**
    
    - Sub-tasks under a work order.
        
    - Example: "Replace Filter" + "Check Refrigerant Levels."
        
3. **Service Appointment (SA)**
    
    - The actual event of dispatching a tech at a specific time/location.
        
    - Example: "Technician visit scheduled for 10 AM, 20th Aug."
        
4. **Service Resource (SR)**
    
    - Field worker or technician.
        
    - Can also represent equipment (ex: repair truck).
        
5. **Service Territory (ST)**
    
    - Geographic coverage zone.
        
    - Used to ensure technicians are only scheduled in areas they cover.
        
6. **Operating Hours**
    
    - Defines available working times for territories/resources.
        
7. **Skills**
    
    - Special abilities (e.g., "Certified Electrician").
        
    - Used in scheduling to ensure correct assignment.
        
8. **Products, Assets & Inventory**
    
    - Products = catalog of items.
        
    - Assets = installed items at customer site.
        
    - Inventory = where products are stocked (warehouse, van, depot).
        

📍 **Mnemonic:**  
**W**ork Order → **A**ppointment → **R**esource → **T**erritory → **S**kills → **I**nventory  
(_“WARTSI” – sounds like “Works I”_)

---

## 2. Scheduling & Optimization

Scheduling is the **heart of FSL**.

### 🔹 Dispatcher Console

- Web-based app for dispatchers (planners).
    
- Features:
    
    - Drag-and-drop appointments onto tech timeline
        
    - Map view (track technicians, jobs)
        
    - Alerts for late/overdue jobs
        
    - Filter by skills, territories, availability
        

### 🔹 Scheduling Policies

Rules that determine **how jobs get assigned automatically**.

Policy criteria include:

- **Match skills** (only skilled techs)
    
- **Availability** (free time slots)
    
- **Location proximity** (nearest tech)
    
- **Service level agreement (SLA)** priority
    

📍 **Example Policy:** Assign jobs within 10 km, matching skill “Fiber Optics,” and respect SLA deadlines.

### 🔹 Work Rules

Extra restrictions to enforce compliance.

- Example: Only assign “High Voltage” jobs to certified electricians.
    

### 🔹 Optimization Engine

- Auto-optimizes schedules for efficiency.
    
- Factors: travel time, skills, workload balance.
    
- Runs as **batch optimization** or **real-time optimization.**
    

---

## 3. Field Service Mobile App

### 🔹 Purpose

Used by technicians to manage their day. Works **online + offline**.

### 🔹 Features

- View daily schedule (appointments)
    
- Get driving directions (map integration)
    
- Log time spent
    
- Capture signatures/photos
    
- Scan barcodes (for inventory/assets)
    
- Update job status: “Accepted → En Route → In Progress → Completed”
    
- Request spare parts or mark inventory used
    

📍 **Offline Mode** is critical → techs can still see jobs and update data without internet, syncing later.

---

## 4. Inventory & Asset Management

### 🔹 Inventory Locations

- Warehouse
    
- Technician’s van
    
- Customer site
    

### 🔹 Inventory Transactions

- **Issue** (move stock from warehouse → van)
    
- **Return** (van → warehouse)
    
- **Consume** (use a part during service)
    

### 🔹 Asset Management

- Track equipment installed at customer’s site.
    
- Assets can have **service history**, warranties, SLAs.
    
- Example: “AC Unit Model 123 installed at Rahman’s home.”
    

---

## 5. Administration & Configuration

As an Admin, you configure:

1. **Service Territories** → Create territories & assign resources.
    
2. **Skills & Certifications** → Create skills, assign to resources.
    
3. **Operating Hours** → Define service hours for territories & resources.
    
4. **Work Types** → Templates for work orders.
    
    - Example: "Standard AC Repair" = estimated duration, required skills, parts.
        
5. **Service Appointment Lifecycle** → Customize statuses (New → Scheduled → Dispatched → Completed → Canceled).
    
6. **Permissions**
    
    - Dispatcher needs access to Console.
        
    - Technicians need Mobile App permissions.
        

---

## 6. Advanced Features

- **Einstein AI for Field Service** → Predict travel times, no-shows.
    
- **Knowledge Articles** in Mobile App → Techs can access repair guides.
    
- **Customer Engagement** → Automated SMS/email reminders + “Track my technician” live ETA.
    
- **Contractor Management** → Manage 3rd-party resources alongside employees.
    
- **Maintenance Plans** → Auto-generate work orders on schedule (like yearly maintenance).
    

---

## 7. End-to-End Flow (Customer Journey)

1. **Customer Case Created**
    
    - Customer calls about a problem.
        
2. **Work Order Generated**
    
    - Created from the case.
        
3. **Service Appointment Created**
    
    - One or more appointments linked to work order.
        
4. **Dispatcher Scheduling**
    
    - Assigns job manually or via optimization.
        
5. **Technician Mobile Execution**
    
    - Accepts job → travels → fixes issue → logs parts used.
        
6. **Service Appointment Completed**
    
    - Status updated → Work Order closed.
        
7. **Follow-up & Billing**
    
    - Triggers invoice, survey, or next maintenance.
        

---

## 8. Exam/Interview-Style Quick Checkpoints

1. **Q:** What is the difference between a Work Order and Service Appointment?
    
    - _Work Order = overall job. Service Appointment = one scheduled visit for that job._
        
2. **Q:** What’s the role of Scheduling Policies vs Work Rules?
    
    - _Policies = how to assign jobs; Rules = restrictions/filters that must be respected._
        
3. **Q:** Why is Offline Mobile App important?
    
    - _Because technicians often work in areas with no internet, so data syncs later._
        
4. **Q:** What is the function of Optimization Engine?
    
    - _Finds the most efficient way to assign and route jobs._
        
5. **Q:** What object tracks customer equipment at their site?
    
    - _Asset._
        

---

✅ **Summary Memory Aid:**

- **FSL = “Right tech, right job, right time, right parts.”**
    
- Think: **Jobs → Assign → Dispatch → Complete → Bill.**
    
