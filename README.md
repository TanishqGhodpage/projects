# LocationAssistant - Enterprise Workforce Tracking System

**LocationAssistant** is a high-performance, real-time workforce management and telemetry solution. It consists of a native Android client for field employees and a robust Node.js backend with a web-based "Command Center" dashboard for supervisors.

The system is designed for **Adaptive Accuracy**, **Battery Efficiency**, and **Real-time Situational Awareness** using WebSocket technology.

---

## 🚀 Key Features

### 📱 Android Client
*   **Adaptive Tracking Engine**: Intelligently switches between `LOW_POWER`, `MEDIUM`, and `HIGH_ACCURACY` modes based on geofence proximity and motion activity (Still/Walking/Vehicle).
*   **Smart Geofencing**: Local, latency-free geofence triggers with hysteresis buffering (15m margin) to prevent state flickering.
*   **Resilient Connectivity**: `SocketManager` singleton ensuring persistent WebSocket connections with auto-reconnect and offline queuing.
*   **Shift Management**: Clock-in/out workflows integrated with the backend.
*   **Diagnostics Terminal**: In-app "Live Terminal" log showing real-time system events, coordinates, and battery stats.
*   **SOS Panic Button**: Immediate high-priority alert transmission.

### 💻 Web Command Center
*   **Real-time Map**: Live visualization of all active personnel using Leaflet.js with "Dark Matter" cartography.
*   **Live Telemetry**: Monitoring of Battery levels, Speed, Motion State (Static/Moving), and Charging status.
*   **Shift Controls**: Admin capability to remotely end shifts or view active duration.
*   **Event Logging**: Detailed chronological log of Geofence entries/exits and SOS alerts.

---

## 🛠️ Technology Stack

### Android (Client)
*   **Language**: Kotlin
*   **UI Framework**: Jetpack Compose (Material3)
*   **Concurrency**: Coroutines & Flow
*   **Networking**: Socket.io Client (`io.socket:socket.io-client`)
*   **Location**: FusedLocationProviderClient (Google Play Services)
*   **Persistence**: Room Database (SQLite)
*   **Background Processing**: Foreground Services & WorkManager

### Backend (Server)
*   **Runtime**: Node.js
*   **Framework**: Express.js
*   **Real-time Engine**: Socket.io
*   **Persistence**: In-memory (Prototype) / JSON File logging

### Dashboard (Web)
*   **Structure**: HTML5 / JavaScript (ES6)
*   **Styling**: Tailwind CSS
*   **Maps**: Leaflet.js + CartoDB Tiles

---

## 🏗️ Architecture

### High-Level Data Flow
1.  **Android App** collects location data via `TrackingService`.
2.  **Adaptive Logic** processes data locally to determine update frequency (e.g., 5s updates inside geofences, 5m updates outside).
3.  **SocketManager** transmits telemetry (`lat`, `lng`, `battery`, `speed`) to the Backend via WebSockets.
4.  **Backend** broadcasts updates to the **Web Dashboard** and logs events.
5.  **Web Dashboard** renders the "Blue Dot" on the map and updates the status panel.

### State Machines
*   **Geofence Logic**: `OUTSIDE` -> `ENTER_PENDING` (3 samples) -> `INSIDE` -> `EXIT_PENDING` (5 samples) -> `OUTSIDE`.
*   **Accuracy Logic**: `LOW_POWER` (Outside >500m) -> `MEDIUM_POWER` (Approaching <500m) -> `HIGH_ACCURACY` (Inside/Near <50m).

---

## ⚙️ Setup & Installation

### Prerequisites
*   Android Studio Ladybug (or newer)
*   Node.js v16+
*   Java JDK 17+

### 1. Backend Setup
1.  Navigate to the backend directory:
    ```bash
    cd backend
    ```
2.  Install dependencies:
    ```bash
    npm install
    ```
3.  Start the server:
    ```bash
    node server.js
    ```
    *   *The server will run on port `3000`.*
    *   *Note your local IP address (e.g., `192.168.1.5`).*

### 2. Android Client Setup
1.  Open the project in **Android Studio**.
2.  Open `app/src/main/java/com/example/locationassistant/data/SocketManager.kt`.
3.  Update the `serverUrl` to match your PC's IP address:
    ```kotlin
    var serverUrl = "http://YOUR_PC_IP:3000"
    ```
4.  Sync Gradle and **Run** the app on an Emulator or Physical Device.

---

## 🕹️ Workflow

1.  **Start Server**: Ensure the Node.js server is running.
2.  **Login**: Open the Android app and enter any Username (auto-provisions a session).
3.  **Start Shift**:
    *   **Option A**: Click the "Alarm" icon on the Web Dashboard to auto-start the app's tracker.
    *   **Option B**: The app automatically connects; tap "Logout" to reset if needed.
4.  **Monitor**: Watch the app's "Terminal" for logs like `[14:30:05] 📍 Loc: ... | VEHICLE`.
5.  **Geofence**: Walk into a simulated geofence area. The app will switch to `HIGH_ACCURACY` and emit "Enter" events.
6.  **End Shift**: Click "End Shift" on the Web Dashboard. The Android app will receive the signal and stop tracking automatically.

---

## 🤝 Contribution
1.  Fork the repository.
2.  Create a feature branch (`git checkout -b feature/AmazingFeature`).
3.  Commit your changes (`git commit -m 'Add some AmazingFeature'`).
4.  Push to the branch (`git push origin feature/AmazingFeature`).
5.  Open a Pull Request.

---

## 📄 License
Distributed under the MIT License. See `LICENSE` for more information.
