# HydroPulseAI
Project Objective:  To monitor, detect, and manage water resources in real-time using AI powered IoT sensors, intelligent alert systems, and a Flutter-based app.  The system should classify alerts into severity levels and take automated  actions where necessary. 

System Overview & Architecture 
Based on the diagrams and your notes, here's the refined modular 
breakdown: 
1. IoT Sensors 
 Flow Meters 
 Pressure Sensors 
 Soil Moisture Probes 
 pH Meters 
 Turbidity Sensors 
 Fire Sensors 
 Door Exit Sensors 
 Smart Fire Extinguisher Trigger 
 Water Level Sensor 


2. Data Flow Pipeline 
Layer 
Component 
A. Data 
Collection 
IoT Device → Gateway → 
Cloud 
Role: Collect and push real-time 
sensor data 

B. API Layer 
Flask/RestAPI (Python) Three APIs: LeakAPI, UsageAPI, 
QualityAPI 

C. Data 
Storage 
MySQL / PostgreSQL 
Store master and 
transactional data 

D. Alert 
System 
Twilio / SMTP / Call API Trigger SMS, email, or call 

E. App Layer 
Flutter 
User dashboard and control 
panel 

API Layer Design 
Use FastAPI for high performance and async support. 
✅ 1. LeakAPI 
 Inputs: Pressure, Flow, Water Level 
 Logic: If pressure drops and flow spikes → Leak Detected 
 Output: status: RED/YELLOW, timestamp, location 
✅ 2. UsageAPI 
 Inputs: Historical Usage + Real-time usage 
 Logic: If current usage > 2x average → Overusage Alert 
 Output: status: YELLOW, timestamp, user_id 
✅ 3. QualityAPI 
 Inputs: pH, Turbidity 
 Logic: Outside ideal range? → Trigger alert 
 Output: status: ORANGE, pH, turbidity, timestamp 



  Phase 1: Setup REST API 
Create Flask App 
Define /leak, /usage, /quality 
Store data in MySQL 
Trigger alerts via alert_handler.py 

  Phase 2: Mobile App (Flutter) 
Setup dashboard 
Connect via http.post() 
Show alert type (Yellow, Orange, Red) 
Push notifications (Firebase or manual) 

  Phase 3: IoT Integration 
Sensors to NodeMCU/RPi 
Send data via HTTP POST 
On red, auto-trigger extinguisher/door sensor 

Functionalities: 
 Modular APIs 
 Central database logging 
 AI-based alert decision system 
 Web based application/ User login Analytics/MIS Reports/Dashboard 
 User-friendly mobile UI 
 End-to-end automation (IoT loop) 

Process: 

 Integrate with Database – Save every POST request into the 
transaction table with timestamp. 
 Connect to Flutter App – Use http package in Flutter to send 
sensor data to these APIs. 
 Trigger IoT Devices – For red alerts, trigger an actuator via 
NodeMCU or Raspberry Pi endpoint. 
 Secure APIs – Add API key/token auth later for safety. 

REST API Workflow 
1. IoT sends data to /api/leak, /api/usage, or /api/quality 
2. Data is stored in Transaction DB with timestamp 
3. Based on logic: - Yellow: SMS/Email - Orange: Missed Call - Red: IoT Action (Auto Triggered) 
4. Alerts triggered via alert_handler 
5. Dashboard & mobile app show the results 

FILE STRUCTURE: 
HydroPulseAI/ 
├── app.py                     
├── config.py                  
├── db.py                      
├── models.py                  
├── alert_handler.py           
├── apis/                      
│   ├── __init__.py            
│   ├── leak_api.py            
│   ├── usage_api.py           
│   └── quality_api.py         
├── utils/                     
│   ├── sms.py                 
│   ├── email_alert.py         
│   └── iot_trigger.py         
├── iot/                       
# Main Flask app file 
# Configuration settings (DB URI, etc.) 
# Database initialization 
# Database models for Owner and Event tables 
# Handles SMS, email, IoT triggers based on severity 
# API folder for modular endpoints 
# API initialization 
# Leak detection API 
# Usage monitoring API 
# Quality monitoring API 
# Utilities folder for reusable functions 
# SMS alert utility (Twilio integration) 
# Email alert utility (SMTP/SendGrid integration) 
# IoT action utility 
# IoT integration folder 
│   ├── nodemcu_code.ino       
# NodeMCU script for sending HTTP requests 
│   └── rpi_script.py          
├── frontend/                  
│   ├── lib/                   
│   │   ├── main.dart          
│   │   └── screens/           
# Raspberry Pi script for IoT control 
# Flutter app folder for UI development 
# Main app logic 
# Flutter main file 
# Screens folder for modular pages 
│   │       ├── home_screen.dart # Home screen for sending data/alerts 
│   └── pubspec.yaml           
└── sql/                       
├── create_tables.sql      
# Flutter package manager 
# SQL scripts folder 
# SQL code for creating tables
