# AgentWeather

An end-to-end serverless machine learning and LangGraph agentic forecasting system that predicts **24-hour-ahead weather and air-quality metrics** (**AQI, temperature (°C), precipitation (mm and rain probability), humidity (%), UV Index, and wind (speed and direction)**) using **Hopsworks**, **GitHub Actions**, and **Gradio / Streamlit**.

---

## Architecture

```text
OpenWeather API + AQICN API
          │
          ▼
[1. Feature & Ingestion Pipeline] ──► Hopsworks Feature Store (Raw, Features, Targets, Logs)
          │                                   ▲               ▲
          ▼                                   │               │
[2. Training & Evaluation Pipeline] ──────────┘               │
    (SARIMAX/Prophet, Ridge/RF, LSTM + SHAP/LIME)           │
          │                                                   │
          ▼                                                   │
  Hopsworks Model Registry ◄── [3. LangGraph Agent Layer] ───┘
                                  ├── Forecast Agent (hourly)
                                  ├── Alert Agent (rules + LLM w/ fallback)
                                  ├── Weather Report Agent (daily & on alert)
                                  └── Drift Agent (data & model drift -> retrain)
                                            │
                                            ▼
                              [4. Deployed Web Dashboard]
                        (Forecasts, News Report, Agent Activity,
                         EDA, SHAP/LIME, Drift & Call Logs)
