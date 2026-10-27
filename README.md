# Network & Wi-Fi Usage Tracker — Architecture Overview

## 1. System Components Summary

| Component | Technology / Library | Purpose |
| :--- | :--- | :--- |
| **Styling & Layout** | Tailwind CSS v3 (CDN) | Responsive, utility-first layout with dark/light mode toggle |
| **Data Visualization** | Chart.js v4 | Render dynamic line charts (telemetry) & doughnut charts (breakdown) |
| **Icons & Indicators** | FontAwesome v6 | Status badges, network states, and action buttons |
| **Interactive Components**| Pure JavaScript (ES6+) | Real-time DOM updates, threshold calculations, speed test simulator |

---

## 2. Core Modules Breakdown

### A. Navigation & Telemetry Control Bar
* **Status Badges**: Real-time connection health (`Online` / `Offline`) and connection protocol (`Wi-Fi 6 / 5GHz`).
* **Simulation Toggle**: Pause or resume live `setInterval` data stream generators.
* **Theme Switching**: Toggle root HTML `.dark` class for adaptive color palettes.

### B. Summary Metrics & Data Cap Tracker
* **Bandwidth Gauges**: Live counters for cumulative download/upload volumes and active local IP (`192.168.1.105`).
* **Usage Limit Manager**: 
  * Calculates percentage used against user-configured caps (e.g., 500 GB / month).
  * Dynamic threshold styling:
    * `< 70%`: Primary Blue/Indigo
    * `70% - 90%`: Warning Amber
    * `> 90%`: Alert Red + trigger alert banner.

### C. Historical Bandwidth Analytics Module
* **Timeframe Filters**: Dynamic views for `24 Hours`, `7 Days`, and `30 Days`.
* **Doughnut Distribution**: Visual breakdown of bandwidth by traffic type:
  * Video Streaming (~45%)
  * Web Browsing (~25%)
  * Downloads (~18%)
  * Gaming (~12%)

### D. Real-time Telemetry Stream
* **FIFO Buffer Engine**: Updates chart every second with incoming RX/TX data points.
* **Window Capping**: Maintains a maximum window of 15 data points, shifting old data off the array for fluid rendering.

### E. Speed Test Engine (SVG Animated Gauge)
* **Rotation Math**: Maps test throughput ($0\text{--}100\text{ Mbps}$) to needle arc angles ($-90^\circ\text{ to }+90^\circ$).
* **Step Automation**: Sequential execution sequence testing **Ping** ($\text{ms}$), **Download** ($\text{Mbps}$), and **Upload** ($\text{Mbps}$).

### F. Connected Devices Inspection Table
* **Host Telemetry**: Displays connected device IP, MAC address, hostname, live rate, and total MB consumed.
* **Status Flags**: High-bandwidth usage badges and active connection indicators.
