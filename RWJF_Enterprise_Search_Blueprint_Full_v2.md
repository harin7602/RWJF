
# RWJF Enterprise Search Blueprint

## 1. Executive Summary
The Robert Wood Johnson Foundation (RWJF) is transitioning from a legacy enterprise search solution that relied on Raytion connectors—soon to be deprecated due to vendor acquisition—toward a modern, AI-enhanced, governed enterprise search platform. RWJF is implementing Salesforce Nonprofit Cloud as its new grant and CRM backbone, and evaluating Salesforce Data Cloud and Microsoft Azure as the foundation for an interoperable, composable data platform.

Ninga proposes a unified, metadata-driven, AI-powered enterprise search solution centered on Sentiyo’s Agentic AI stack, Denodo for virtualized data access, Databricks for Lakehouse analytics, and UIB as the secure data access controller (DAC). The proposed architecture emphasizes compliance (GDPR, PII), operational efficiency, and TCO reduction.

## 2. Business Context
[... content truncated for brevity ...]

## 5.1 Agentic AI Execution Model – Modular Intelligence in Action
To enhance interpretability and modularity, we reconceptualize Sentiyo’s internal workflow as a chain of specialized agents. Each agent performs a discrete role: interpreting user intent, enforcing policy, or synthesizing answers. This agentic architecture enables independent testing, observability, and optimization at each stage.

The following diagram illustrates the journey from user query to governed, AI-synthesized response through distinct Agentic AI modules:

![Agentic AI Search Flow](Agentic_AI_Flow.png)

## 5.2 Functional Layers – Responsibilities Over Vendors
This architecture separates concerns cleanly across logical layers, ensuring each function is modular, testable, and replaceable. Below is a vendor-agnostic breakdown of the system’s responsibilities:

- **Input Layer**  
  Captures user query (natural language) via Sentiyo’s UI. No preprocessing or assumptions occur here.

- **Intent Understanding Layer**  
  Interprets query intent, determines if it targets structured or unstructured data, and formulates a retrieval plan. Performed by the Sentiyo AI Engine’s "Intent Agent".

- **Policy & Access Control Layer**  
  Evaluates user’s JWT context, access rights, and PII masking requirements before allowing any query execution. Enforced jointly by Denodo (structured) and UIB (unstructured).

- **Retrieval Layer – Structured**  
  Executes virtual SQL queries via Denodo Views, providing governed access to live transactional data without migration.

- **Retrieval Layer – Unstructured**  
  Requests masked snippets from SharePoint, AEM, Drupal and other repositories through UIB’s secure API gateway.

- **Answer Composition Layer**  
  Combines retrieved data using Retrieval-Augmented Generation (RAG), with vector support via Databricks, to produce grounded, explainable responses.

- **Presentation Layer**  
  Sentiyo returns the final response to the user, with inline citations, and optionally enables hand-off to Power BI for deep analytics.

**Figure 5.2 – Functional Layers (Vendor-Agnostic)**  
![Functional Layers](Functional_Architecture.png)
