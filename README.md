# RK AI Trading Analyzer

Educational market-analysis dashboard with a Python FastAPI backend, responsive web frontend, and Android wrapper build workflow.

## Important limitations
- Paper/research mode only. No brokerage connection or real-money order execution.
- Signals are heuristic indicator summaries, not guaranteed predictions or personal financial advice.
- Public Binance spot candles are used for crypto markets. OTC EUR/CHF quotes from binary-options platforms are **not** available from this provider and are not approximated.
- 1m, 5m and 15m candles are supported by Binance. 2m candles are built by aggregating two consecutive 1m candles.
- The initial app is a working starter. Add and verify licensed providers before enabling stocks, indices or FX.

## Run locally
Requires Python 3.11+ and Node.js 20+.

### Backend
```bash
cd backend
python -m venv .venv
# Windows: .venv\Scripts\activate
# Linux/macOS/Termux: source .venv/bin/activate
pip install -r requirements.txt
cp .env.example .env
uvicorn app.main:app --host 0.0.0.0 --port 8000 --reload
```
API docs: http://127.0.0.1:8000/docs

### Frontend
In a second terminal:
```bash
cd frontend
npm install
cp .env.example .env
npm run dev
```
Set `VITE_API_BASE_URL=http://127.0.0.1:8000` in `frontend/.env`.

## Android APK
The Android project is generated from the same frontend using Capacitor:
```bash
cd frontend
npm install
npm run build
npx cap add android
npx cap sync android
npx cap open android
```
Build a debug APK in Android Studio, or use the included GitHub Actions workflow. To use Actions, push this project to a GitHub repository and run **Build Android APK**. The workflow uploads the debug APK as an artifact. For a cloud build, set `VITE_API_BASE_URL` to a publicly reachable HTTPS backend URL in the workflow repository variables.

## Environment
Backend: `backend/.env.example`
Frontend: `frontend/.env.example`

Never put provider secrets in frontend variables. The included crypto provider uses public market endpoints and needs no API key.

## Supported data
- `BTCUSDT`, `ETHUSDT`, `BNBUSDT`, `SOLUSDT`, `XRPUSDT`
- Binance public spot market data, with a provider status label.
- If the provider is unavailable, the API returns an error; it does not invent candles.

## Tests
```bash
cd backend
pip install -r requirements.txt
pytest
```
