# VIKAS SMARTFLOW — Ideathon Prototype

A responsive React web application prototype for tracking automotive heat exchangers from raw material through production, quality, inventory, warehouse and dispatch. All records are synthetic demo data and are not actual Vikas Group data.

## Requirements
- Node.js 20+ (recommended)
- VS Code
- Internet access for the optional Google Fonts stylesheet; the app itself does not require an external data service.

## Run locally
1. Extract the ZIP and open the `vikas-smartflow` folder in VS Code.
2. Open **Terminal → New Terminal**.
3. Install dependencies:
   ```bash
   npm install
   ```
4. Start the development server:
   ```bash
   npm run dev
   ```
5. Open the local URL Vite prints (usually `http://localhost:5173`).

## Demo login
All demo passwords are `demo123`.

| Username | Role |
|---|---|
| `worker` | Worker |
| `quality` | Quality Engineer |
| `warehouse` | Warehouse Operator |
| `supervisor` | Supervisor |
| `manager` | Manager |
| `admin` | Administrator |

The quick-login buttons fill in and sign in immediately.

## Included working prototype flows
- Responsive login, role-aware sidebar navigation, and sign out.
- Dashboard KPI cards, Recharts visualizations, alert navigation, notifications and activity timeline.
- Product list search, product selection and QR generation on the passport.
- Manual QR/product ID search. The camera button explains the secure-context requirement; a production camera scanner still needs to be connected to the UI.
- Digital Product Passport with QR code, lifecycle, synthetic product/material/production/quality/location/dispatch data, activity events, and browser print/save-to-PDF.
- Production start/complete actions.
- Quality checklist; failed checks place the product on hold, passed checks update its quality state.
- Inventory and warehouse location assignment.
- Dispatch verification checks product existence, order association, quality pass, inventory location, and duplicate dispatch before allowing confirmation.
- Simulated machine monitoring, AI risk recommendations, sustainability and analytics screens.
- Reset demo data in Settings.

## Data and architecture notes
This initial delivery uses local synthetic data and `localStorage` for product updates, so it can run without a server or private enterprise systems. Orders, machine data and other initial records are demo fixtures in `src/App.jsx`. This is intentionally an initial frontend prototype, **not** a production-secure manufacturing system. Authentication is demo-only, not suitable for real users. A later backend phase should move records and workflow validation into FastAPI + SQLite/PostgreSQL, add role-based authorization on every API endpoint, persist all entity tables, and connect a trained scikit-learn model and camera QR scanner.

## Database schema direction
Suggested backend tables: `users`, `products`, `production`, `quality`, `inventory`, `orders`, `dispatch`, `machines`, `ai_predictions`, and `activity_events`. Use foreign keys between product, order and workflow records; validate product state transitions server-side and write an activity event for every successful state change.

## Suggested next implementation phase
1. Add FastAPI API routes and SQLite persistence.
2. Add a browser camera QR scanner with `html5-qrcode` and handle camera permissions.
3. Add synthetic-data generator (500+ records) and train/test a RandomForestClassifier and IsolationForest in Python; expose model metrics and predictions through API endpoints.
4. Add tests for invalid product transitions, failed quality, order mismatch, and duplicate dispatch.

## Prototype disclaimer
Vikas SmartFlow — Ideathon Prototype | Synthetic Demonstration Data. Prototype demonstration using synthetic data. Actual industrial deployment requires integration with authorized enterprise systems, validated process limits, cybersecurity controls and engineering approval.


## AI Risk Center update (product-aware live demo)

The AI Risk Center now lets the operator choose any synthetic product and an order. Its displayed risk score and contributing factors are derived from the selected product's progress/quality, the assigned machine's simulated temperature/current/status, and the selected order's completion value. Because machine telemetry refreshes every 2.5 seconds, the risk display refreshes while the app is open. Recommendations adapt to machine alerts, pending production, and quality holds. The Product Passport can be opened directly for the selected product.

All values remain synthetic demonstration data. This is a deterministic demonstration risk heuristic, not validated ML inference or live factory data.

### Run locally

```powershell
npm install
npm run dev
```

Open the local URL printed by Vite. The project requires a working internet connection for the first dependency installation unless dependencies are already cached.


## Navigation and camera notes

- The left navigation is hidden initially. Use the hamburger button to open it, then choose any available prototype section; use the close button or backdrop to dismiss it.
- The QR Scanner uses the browser camera through `html5-qrcode`. Open the app on `http://localhost:5173` during local development or deploy it on HTTPS, allow camera permission in the browser, and close other apps that may be using the camera. Manual product ID search remains available as a fallback.
- All sections are visible in the prototype across demo roles so the full interface can be reviewed. Synthetic readings are illustrative only and are not connected to live factory hardware.


## Reporting window selector (prototype)

A shared reporting-window selector is available in the top bar across the application. It changes trend-chart aggregation labels to hourly (today), daily (this week), weekly (this month), or monthly (last 90 days) views, and the selection is shared across Dashboard, Sustainability, and Analytics. Telemetry continues to refresh in the running browser. The included dataset is synthetic; historical time-bucket values are representative prototype views, not a replacement for timestamped plant historian data. A production deployment should query stored timestamped records from the MES/ERP/SCADA data source for accurate date-window filtering.

## QR scanner test cards
- `qr-codes/MATCHED_product_HEX-001.png` encodes `HEX-001`, a registered demo product. Scan it to see the product lifecycle and status.
- `qr-codes/UNMATCHED_test_HEX-999.png` encodes `HEX-999`, which is not registered in the default demo records. Scan it to see the unmatched result.

Open the app on `http://localhost:5173` or an HTTPS deployment, sign in, open **QR Scanner**, start the camera, and hold one printed or on-screen QR code steady inside the scan frame. The scanner stops after reading a QR code and displays either the matched product record or an unmatched warning. Camera access depends on browser permission and available camera hardware. If `smartflow-products` has been customized in browser local storage, use a product ID that exists in that local product list.
