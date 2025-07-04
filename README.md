# 📡 SonarAssessment — Real-time Dashboard with WebSocket & AI Integration

A high-performance React + TypeScript dashboard for visualizing real-time streaming data, supporting collaboration, and integrating AI-powered insights.

---

## 🔍 Overview

SonarAssessment is a **real-time monitoring dashboard** for **500+ client websites**. Built for scalability and speed, it features:

- ⚡ **WebSocket-based real-time data streaming**  
- ⚛️ **React dashboard** with **data virtualization** for large datasets  
- 🤖 **AI Assistant** for natural-language chart queries  
- 📤 **PDF/CSV export** for reporting  
- 👥 **Multi-user collaboration** with smart caching and state sync

---

## 🚀 Features

- 📊 **Live Dashboard**: Streaming metrics (traffic, latency, errors) per website
- 📡 **Custom `useWebSocket` Hook**: Low-memory, continuous data ingestion
- 🧠 **AI Assistant (OpenAI)**: Conversational insights from dashboard data
- 🗃️ **Data Virtualization**: Efficient rendering of large lists/charts
- 📤 **Export**: CSV/PDF export of chart/table data
- 👥 **Multi-user**: Collaborative sessions supported
- 🧠 **Caching & Pruning**: Smart memory management for streaming data

---

## ⚙️ Tech Stack

| Category           | Stack                                         |
|--------------------|-----------------------------------------------|
| **Frontend**       | React + TypeScript + Vite                     |
| **Realtime Data**  | WebSocket (`wss://...`)                       |
| **Styling**        | Tailwind CSS                                  |
| **Charts**         | Recharts / Chart.js                           |
| **Exports**        | jsPDF / FileSaver.js / papaparse              |
| **Dev Tools**      | ESLint, Prettier, Husky                       |

---

## 🛠️ Installation

Clone and install dependencies:

```bash
git clone https://github.com/ShrinidhiKaranth16/sonarAssesment.git
cd sonarAssesment
npm install
```

---

## 📜 Scripts

Development and production commands:

```bash
# Start dev server
npm run dev

# Format code
npm run format

# Lint code
npm run lint

# Build for production
npm run build

# Preview production build
npm run preview
```

---

## 🧾 Project Structure

```bash
sonarAssesment/
├── src/
│   ├── components/         # Dashboard UI
│   ├── hooks/              # Custom hooks (e.g. useWebSocket)
│   ├── services/           # API interaction (e.g. useDashboardAI)
│   ├── types/              # TypeScript interfaces
│   ├── pages/              # Dashboard and app pages
│   └── App.tsx             # Main app
├── public/                 # Static assets
├── .env                    # Environment variables
├── vite.config.ts          # Vite config
└── README.md               # Documentation
```

---

## 🔌 WebSocket System

The `useWebSocket` hook connects to:

```
wss://sonar-lab-server-8881cb834ac4.herokuapp.com/
```

---

## 📦 Data Types

Core real-time analytics models:

### `Page`

```ts
interface Page {
  path: string;      // e.g., "/home", "/product"
  views: number;     // total views for the page
}
```

### `UserFlow`

```ts
interface UserFlow {
  from: string;      // originating path
  to: string;        // destination path
  count: number;     // number of users transitioning
}
```

### `DataPoint`

```ts
interface DataPoint {
  timestamp: string;               // ISO timestamp
  siteId: string;                  // Unique site identifier
  siteName: string;                // Human-readable name
  pageViews: number;               // Total views in this interval
  uniqueVisitors: number;          // Distinct visitors in the interval
  bounceRate: number;              // Bounce rate in %
  avgSessionDuration: number;      // Avg session duration (seconds)
  topPages: Page[];                // Top pages by views
  performanceMetrics: {
    loadTime: number;                     // Load time (ms)
    firstContentfulPaint: number;         // FCP (ms)
    largestContentfulPaint: number;       // LCP (ms)
  };
  userFlow: UserFlow[];            // Page-to-page user movement
}
```

---

## 📈 Usage

Interfaces are used in:

- `useWebSocket` → for real-time data
- Charts/Tables → for metrics visualization

---

## 📡 useWebSocket Hook

**Purpose:**  
Streams real-time `DataPoint` objects from a WebSocket endpoint. Designed for high-frequency, low-memory data ingestion.

**Usage:**

```tsx
const data = useWebSocket();
```

Returns an auto-updating array of the latest `DataPoint` items.

**WebSocket Endpoint:**

```
wss://sonar-lab-server-8881cb834ac4.herokuapp.com/
```

**Behavior:**

- Connects on mount (`useEffect`)
- Parses each message into a `DataPoint`
- Stores only the last 1000 entries (`Array.slice(-999)`)
- Handles errors gracefully
- Closes socket on unmount

**Example Integration:**

```tsx
const Dashboard = () => {
  const liveData = useWebSocket();
  return <Chart data={liveData} />;
};
```

---

## 📊 Dashboard Component

**Purpose:**  
Renders a real-time analytics dashboard for multiple websites using streaming WebSocket data. Aggregates and visualizes:

- Page views over time
- Top-performing pages
- User navigation flow

**Features:**

- ✅ Real-time updates
- ✅ Multi-site switching
- ✅ Aggregated charts
- ✅ Modular, lightweight

**Charts Rendered:**

| Chart                | Description                                 |
|----------------------|---------------------------------------------|
| PageViewsLineChart   | Time-series of page views per site          |
| TopPagesBarChart     | Views per page path (bar chart)             |
| UserFlowHeatMap      | User page transitions (heatmap)             |

**State Management:**

- `data`: Streamed from `useWebSocket`
- `uniqueSites`: All distinct sites from data
- `selectedSite`: Current site being analyzed
- `lineChartData`: Timestamp vs. page views
- `barChartData`: Aggregated views per page
- `heatMapData`: Transition flow between pages

**Key Logic:**

- Initializes first site on load
- Aggregates/transforms `DataPoint[]` by selected site
- Supports site switching via button group
- Uses `useEffect` to:
  - Watch/parse new streaming data
  - Update visualizations on site change

**Enhancement Ideas:**

- Persist last selected site (localStorage)
- Add chart export/download
- Add search/filter to site selector
- Add loading/fallback state for data

---

<!-- Optionally add a screenshot here to show the dashboard UI -->
