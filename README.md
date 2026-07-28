# 🗺️ aladhan app

A cross-platform Islamic platform providing accurate, real-time prayer timings, a dynamic next-prayer countdown, and an interactive Hijri calendar. Built with a unified API architecture, this project begins as a responsive web application and scales into a native mobile app.

---

## 🚀 Project Roadmap

The development of this project is divided into two distinct phases to ensure a smooth transition from web to mobile deployment:

*   **Phase 1 (Current):** Web Application built using **Vue.js 3** (Composition API + Vite).
*   **Phase 2 (Upcoming):** Cross-platform Mobile Application built using **Flutter**.

---

## 🛠️ Tech Stack & Architecture

### Frontend (Web)
*   **Framework:** Vue 3 (Vite, Composition API)
*   **HTTP Client:** Axios
*   **Styling:** Modern CSS Grid / Flexbox (Responsive Design)

### Mobile (Upcoming)
*   **Framework:** Flutter (Dart)
*   **Networking:** `http` or `dio` package

### Data Layer (API)
This application operates entirely client-side/serverless in its initial MVP phase, pulling data directly from the public **AlAdhan API**.
*   **Default Calculation Method:** Method 3 (Egyptian General Authority of Survey)
*   **Core Parameters:** Lat/Long Geolocation or City/Country Queries

---

## ✨ Features

- [x] **Real-time Synchronization:** Fetches accurate local prayer times (`Fajr`, `Sunrise`, `Dhuhr`, `Asr`, `Maghrib`, `Isha`).
- [ ] **Dynamic Countdown:** A live countdown timer displaying hours, minutes, and seconds until the next prayer.
- [ ] **Hijri Calendar View:** Monthly view detailing Islamic dates alongside Gregorian counterparts.
- [ ] **Location Detection:** Automatic localized matching via browser/device geolocation.

---

## 📦 Getting Started (Phase 1: Web App)

### Prerequisites
Make sure you have [Node.js](https://nodejs.org/) installed on your machine.

### Installation

1. Clone the repository:
```bash
   git clone [https://github.com/tahar-irki/aladhan.git]
   cd aladhan