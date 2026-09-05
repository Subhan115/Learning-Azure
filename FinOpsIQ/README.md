
# FinOpsIQ – AI-Powered Azure Cost Anomaly Detection

**FinOpsIQ** is an intelligent, serverless FinOps platform that helps cloud teams detect unusual Azure cost spikes early and understand them in simple, business-friendly language. It automatically monitors daily spend, uses AI to explain what caused each spike and what actions to take, and generates a weekly one-page summary of the top cost risks and savings opportunities.

______________________________________________________________________

## 🎯 Purpose

The goal of FinOpsIQ is to **reduce wasted cloud spend** and make cost insights **clear enough for both engineers and managers to act on quickly**—without requiring manual budget guesses or constant monitoring.

______________________________________________________________________

## ✨ Key Features

- **Smart Spike Detection**: Alerts when daily spend exceeds X% above your normal baseline (user-defined threshold).
- **AI-Powered Explanations**: Plain-language insights like *"Spend is 67% above baseline due to VM scale-out in prod-eastus. Action: Review autoscale rules."*
- **Weekly Manager Scorecard**: 1–2 minute read summarizing top cost risks, trends, and savings opportunities.
- **Multi-Tenant Ready**: Single deployment monitors multiple Azure subscriptions with per-tenant configuration.
- **Production-Grade Security**: Managed identity, Key Vault secrets, 90-day data retention, full audit trail.

______________________________________________________________________

## 🏗️ Architecture

```
Azure Cost Management → Blob Storage (daily CSV export)
                              ↓
                    Azure Function (Python)
                    - Detects spikes (user-defined %)
                    - Calls AI Foundry for explanations
                              ↓
                    Logic Apps → Email/Teams alerts
                    Blob Storage → Anomaly records + weekly scorecards
```

**Core Services**: Azure Function App, Blob Storage, AI Foundry, Key Vault, Logic Apps, Application Insights, Entra ID (Managed Identity).


flowchart TB
    subgraph User["👤 User Configuration"]
        Config[("📝 subscriptions.json<br/>spike %, baseline window")]
    end

    subgraph Azure["☁️ Azure Cloud"]
        subgraph CostMgmt["💰 Azure Cost Management"]
            Export[("📥 Daily CSV Export<br/>(24h lag)")]
        end

        subgraph Storage["🗄️ Blob Storage"]
            CostCSV[("📄 cost_exports/YYYY-MM-DD.csv")]
            Anomalies[("📄 anomalies/YYYY-MM-DD.json")]
            Scorecards[("📄 scorecards/weekly_YYYY-MM-DD.md")]
            AppConfig[("⚙️ config/subscriptions.json")]
        end

        subgraph Compute["⚡ Azure Function App<br/>(Python)"]
            Trigger[("⏰ Daily Timer<br/>6:00 AM PKT")]
            SpikeDetect["🔍 Spike Detection<br/>(baseline vs actual)"]
            AIIntegration["🤖 AI Foundry Call<br/>(generate explanation)"]
            WeeklyJob["📊 Weekly Scorecard<br/>(aggregate top spikes)"]
        end

        subgraph AI["🧠 Azure AI Foundry"]
            Model[("💬 Cheapest Model<br/>(Phi-3 / GPT-3.5)")]
        end

        subgraph Security["🔐 Security Layer"]
            KeyVault[("🔑 Key Vault<br/>(AI API keys)")]
            ManagedID[("🆔 Managed Identity<br/>(Entra ID)")]
        end

        subgraph Notify["📧 Azure Logic Apps"]
            Alert[("⚠️ Instant Alert<br/>(email/Teams)")]
            WeeklyReport[("📬 Weekly Scorecard<br/>(managers)")]
        end

        subgraph Monitor["📈 Application Insights"]
            Logs[("📋 Execution Logs")]
            Metrics[("📊 Custom Metrics")]
        end
    end

    Config --> AppConfig
    Export --> CostCSV
    CostCSV --> SpikeDetect
    AppConfig --> SpikeDetect
    SpikeDetect --> AIIntegration
    AIIntegration --> Model
    Model --> AIIntegration
    KeyVault -.-> AIIntegration
    ManagedID -.-> Storage
    ManagedID -.-> KeyVault
    ManagedID -.-> Model
    AIIntegration --> Anomalies
    SpikeDetect --> Alert
    WeeklyJob --> Scorecards
    Scorecards --> WeeklyReport
    Compute --> Logs
    Compute --> Metrics

    style User fill:#e1f5ff
    style Azure fill:#f0f0f0
    style CostMgmt fill:#fff4e6
    style Storage fill:#e8f5e9
    style Compute fill:#fff3e0
    style AI fill:#f3e5f5
    style Security fill:#ffebee
    style Notify fill:#e3f2fd
    style Monitor fill:#f5f5f5
    
______________________________________________________________________

## 📊 How It Works

1. **Daily**: Azure Cost Management exports cost data to Blob Storage.
2. **Function Trigger**: Python Function reads new CSV, calculates baseline (7-day avg), detects spikes.
3. **AI Analysis**: Azure AI Foundry generates plain-language explanation + action items.
4. **Alerting**: Logic Apps sends instant email/Teams notification if spike detected.
5. **Weekly**: Aggregated scorecard sent to managers (top risks + savings opportunities).

______________________________________________________________________

## 🛡️ Security

- **Managed Identity**: Function accesses all Azure services without secrets.
- **Key Vault**: Stores AI API keys securely.
- **RBAC**: Least-privilege access via Entra ID.
- **Data Retention**: 90-day automatic cleanup via Blob lifecycle policies.

______________________________________________________________________

## 📈 Career \& Learning Value

This project demonstrates:

- **Serverless architecture** (Azure Functions, Logic Apps)
- **Infrastructure as Code** (Terraform, DRY modules)
- **AI integration** (Azure AI Foundry for business insights)
- **Multi-tenant design patterns** (enterprise-scale thinking)
- **FinOps domain expertise** (cost optimization, anomaly detection)

Perfect for **Azure cloud engineers**, **platform engineers**, and **DevOps professionals** building portfolio-worthy, production-grade projects.

______________________________________________________________________

## 📝 License

MIT License – feel free to use, modify, and learn from this project.

______________________________________________________________________

## 🤝 Contributing

Issues and PRs welcome! This is a learning project—feedback and improvements are encouraged.

______________________________________________________________________

**Built with ❤️ for the Azure community**\
*Reducing cloud waste, one spike at a time.*

