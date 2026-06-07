# SafeZone AI - Predictive Blind-Spot Accident Prevention System

SafeZone AI is a real-time accident prevention and monitoring system designed for heavy vehicles such as excavators, construction vehicles, and campus safety vehicles. The system detects objects or workers near the vehicle blind spot, calculates risk level, updates live status to Firebase, and displays alerts on a web dashboard.

The project supports both:

* Demo Test Mode
* Real Arduino Mode

---

## Project Overview

SafeZone AI continuously monitors the distance between a vehicle and nearby objects or workers. Based on the distance and approach speed, the system calculates the risk level as:

* SAFE
* WARNING
* DANGER
* CRITICAL

When the risk level becomes critical, the system activates an emergency stop status and sends live data to Firebase. The dashboard displays live vehicle status, supervisor alerts, AI prediction details, emergency messages, analytics, and black box accident logs.

---

## Main Features

### 1. Live Vehicle Monitoring

The dashboard shows real-time vehicle data such as:

* Vehicle ID
* Distance from object
* Risk level
* Approach speed
* Motor status
* Location name
* Alert status
* Accident location map

---

### 2. AI Risk Calculation

The system calculates risk using distance and approach speed.

Risk conditions:

```txt
Distance <= 5 cm or Approach Speed >= 9  → CRITICAL
Distance <= 10 cm or Approach Speed >= 6 → DANGER
Distance <= 20 cm or Approach Speed >= 3 → WARNING
Above these values                       → SAFE
```

---

### 3. Demo Test Mode

Demo mode is used for testing without Arduino hardware.

In demo mode:

* Random distance values are generated.
* Random critical alerts are created.
* Firebase is updated every 5 seconds.
* Dashboard receives live updates from Firebase.

To enable demo mode:

```js
const USE_TEST_MODE = true;
```

---

### 4. Real Arduino Mode

Arduino mode is used when real sensor data is coming from Arduino through serial communication.

To enable Arduino mode:

```js
const USE_TEST_MODE = false;
```

Arduino serial port used:

```js
path: "COM12"
baudRate: 9600
```

Arduino should send data in this format:

```txt
distance,riskLevel,alertStatus,approachSpeed,motorStatus
```

Example:

```txt
8,DANGER,Danger Zone,6,SLOW
```

---

### 5. Firebase Realtime Database

The backend updates Firebase in real time.

Firebase database URL:

```txt
https://safezone-ai-17dc9-default-rtdb.firebaseio.com/
```

Database paths used:

```txt
liveStatus
accidentLogs
```

`liveStatus` stores the latest vehicle status.

`accidentLogs` stores warning, danger, and critical incident history.

---

### 6. Supervisor Alert System

The dashboard shows alert messages based on risk level.

Examples:

```txt
SAFE     → No object detected in danger zone.
WARNING  → Object near blind spot.
DANGER   → Object very close to vehicle blind spot.
CRITICAL → Worker in blind spot. Vehicle stopped automatically.
```

---

### 7. Emergency Response Message

When a critical alert occurs, the dashboard generates an emergency response message containing:

* Vehicle ID
* Distance
* Location
* Accident map link
* Recommended action

---

### 8. AI Prediction Engine

The AI prediction engine calculates:

* AI Risk Score
* Collision countdown
* Priority level
* Recommended action

Priority levels:

```txt
SAFE     → LOW
WARNING  → MEDIUM
DANGER   → HIGH
CRITICAL → EMERGENCY
```

Collision countdown is calculated using:

```txt
Time to Collision = Distance / Approach Speed
```

---

### 9. Black Box Accident Logs

The dashboard stores and displays accident logs like a black box system.

Each log contains:

* Vehicle ID
* Distance
* Risk level
* Approach speed
* Location
* Map link
* Alert status
* Motor status
* Time

---

### 10. Safety Analytics

The dashboard calculates:

* Total alerts
* Warning alerts
* Danger alerts
* Critical alerts
* Most unsafe zone

This helps supervisors identify dangerous areas.

---

## Technologies Used

### Backend

* Node.js
* Express.js
* CORS
* Firebase Admin SDK
* SerialPort
* Readline Parser

### Frontend

* HTML
* CSS
* JavaScript
* Firebase Web SDK
* Firebase Realtime Database

### Hardware Support

* Arduino
* Ultrasonic sensor or distance sensor
* Motor control system
* Serial communication

---

## Folder Structure

```txt
safezone-ai/
│
├── backend/
│   ├── server.js
│   ├── serviceAccountKey.json
│   ├── package.json
│   └── node_modules/
│
├── frontend/
│   ├── index.html
│   ├── script.js
│   └── style.css
│
└── README.md
```

---

## Backend Setup

### 1. Create Backend Folder

```bash
mkdir backend
cd backend
```

### 2. Initialize Node Project

```bash
npm init -y
```

### 3. Install Required Packages

```bash
npm install express cors firebase-admin serialport @serialport/parser-readline
```

### 4. Add Firebase Service Account Key

Download the Firebase Admin SDK service account key from Firebase Console.

Rename the file as:

```txt
serviceAccountKey.json
```

Place it inside the `backend` folder.

---

## Running the Backend

Run the backend server using:

```bash
node server.js
```

Expected output in demo mode:

```txt
Running in DEMO TEST MODE
Server running on http://localhost:5000
Firebase Updated:
```

Expected output in Arduino mode:

```txt
Running in ARDUINO MODE
Arduino Connected on COM12
Server running on http://localhost:5000
```

---

## Backend API Routes

### Home Route

```txt
GET /
```

Response:

```txt
SafeZone AI Backend Running
```

### Live Status Route

```txt
GET /liveStatus
```

Response example:

```json
{
  "vehicleId": "Excavator-01",
  "distance": 8,
  "riskLevel": "DANGER",
  "approachSpeed": 6,
  "locationName": "Campus Zone A",
  "latitude": "12.9716",
  "longitude": "77.5946",
  "mapLink": "https://maps.google.com/?q=12.9716,77.5946",
  "alertStatus": "Danger Zone",
  "motorStatus": "SLOW",
  "time": "8/6/2026, 10:30:00 AM"
}
```

---

## Frontend Setup

Create a folder named `frontend`.

Inside it, create these files:

```txt
index.html
script.js
style.css
```

Paste the HTML code inside `index.html`.

Paste the JavaScript code inside `script.js`.

Paste the CSS code inside `style.css`.

---

## Running the Frontend

You can run the frontend by opening:

```txt
index.html
```

in a browser.

Recommended method:

Use VS Code Live Server extension.

Steps:

1. Open the `frontend` folder in VS Code.
2. Right-click on `index.html`.
3. Click **Open with Live Server**.

---

## Firebase Data Structure

The Firebase Realtime Database will contain:

```txt
safezone-ai-17dc9-default-rtdb
│
├── liveStatus
│   ├── vehicleId
│   ├── distance
│   ├── riskLevel
│   ├── approachSpeed
│   ├── locationName
│   ├── latitude
│   ├── longitude
│   ├── mapLink
│   ├── alertStatus
│   ├── motorStatus
│   └── time
│
└── accidentLogs
    ├── logId1
    ├── logId2
    └── logId3
```

---

## How the System Works

1. Backend starts running.
2. In demo mode, random distance values are generated.
3. In Arduino mode, sensor values are read from serial port.
4. Risk level is calculated.
5. Latest data is saved to Firebase under `liveStatus`.
6. Warning, danger, and critical data are saved under `accidentLogs`.
7. Frontend reads Firebase data in real time.
8. Dashboard updates live vehicle details.
9. AI prediction engine calculates risk score and collision countdown.
10. Supervisor alert and emergency response messages are displayed.
11. Black box logs and analytics are updated automatically.

---

## Risk Level Logic

### SAFE

Condition:

```txt
Distance > 20 cm and Approach Speed < 3
```

Action:

```txt
Vehicle continues running.
```

---

### WARNING

Condition:

```txt
Distance <= 20 cm or Approach Speed >= 3
```

Action:

```txt
Alert driver and monitor blind spot.
```

---

### DANGER

Condition:

```txt
Distance <= 10 cm or Approach Speed >= 6
```

Action:

```txt
Slow down vehicle and prepare to stop.
```

---

### CRITICAL

Condition:

```txt
Distance <= 5 cm or Approach Speed >= 9
```

Action:

```txt
Stop vehicle immediately and send rescue team.
```

---

## Arduino Data Format

The Arduino should send serial data in this format:

```txt
distance,riskLevel,alertStatus,approachSpeed,motorStatus
```

Example safe data:

```txt
30,SAFE,No Alert,0,RUNNING
```

Example warning data:

```txt
18,WARNING,Object Near,3,RUNNING
```

Example danger data:

```txt
9,DANGER,Danger Zone,6,SLOW
```

Example critical data:

```txt
3,CRITICAL,Emergency Stop Activated,10,STOPPED
```

---

## Important Configuration

### Change COM Port

If your Arduino is connected to another port, change this line in `server.js`:

```js
path: "COM12"
```

Example:

```js
path: "COM5"
```

---

### Change Demo or Arduino Mode

For testing without Arduino:

```js
const USE_TEST_MODE = true;
```

For real Arduino sensor input:

```js
const USE_TEST_MODE = false;
```

---

## Output Screens

The dashboard contains:

1. Live Vehicle Status
2. Supervisor Alert
3. AI Safety Analytics
4. Emergency Response Message
5. AI Prediction Engine
6. Black Box Accident Logs

---

## Future Enhancements

* Add user login system
* Add separate admin and supervisor roles
* Add SMS alert system
* Add email alert system
* Add buzzer control
* Add real motor stop relay integration
* Add multiple vehicle monitoring
* Add GPS module support
* Add camera-based blind spot detection
* Add AI-based object classification
* Add mobile app for supervisors

---

## Project Title

SafeZone AI - Predictive Blind-Spot Accident Prevention System

---

## Conclusion

SafeZone AI is a real-time safety monitoring system that helps prevent blind-spot accidents in construction sites, campuses, and industrial areas. It uses sensor data, Firebase Realtime Database, AI-based prediction logic, and a live dashboard to detect danger early and support quick emergency response.
