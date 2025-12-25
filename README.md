# Paycraft
%% PayCraft high-level architecture
flowchart LR
  subgraph Clients
    A[Client App / Server]
  end

  A -->|HTTPS REST/gRPC| API[API Gateway & Auth]
  API --> Orch[Orchestration / Payment Orchestrator]
  Orch --> Router[Smart Routing Engine]
  Router -->|choose rail| Adapter1[M-Pesa Adapter]
  Router --> Adapter2[Airtel Money Adapter]
  Router --> Adapter3[Card Acquirer Adapter]
  Router --> Adapter4[Bank Adapter]
  Adapter1 --> Rails1[External Payment Rail]
  Adapter2 --> Rails2[External Payment Rail]
  Adapter3 --> Acquirer[Card Networks / Issuers]
  Adapter4 --> BankSystems[Bank Networks]

  Orch --> Ledger[Transactional Ledger]
  Orch --> FX[FX & Settlement Engine]
  FX --> Settlement[Settlement & Reconciliation]
  Ledger --> DW[Data Warehouse / BI]
  Settlement --> Ledger

  Orch --> Webhooks[Event Bus & Webhooks]
  Webhooks --> A
  Orch --> Payout[Payout Engine]
  Payout --> Settlement

  Orch --> Compliance[KYC / AML / Fraud Service]
  Compliance --> Orch

  subgraph PlatformTools
    SDKs[SDKs & CLI]
    Sandbox[Sandbox / Test Environment]
    Dashboard[Merchant Dashboard & Analytics]
    Observability[Monitoring & Tracing]
  end

  A <-->|SDKs / docs| SDKs
  Dashboard --> DW
  Observability --> Orch
  Observability --> API
