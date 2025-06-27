---
title: "AI‑Driven Enterprise Search Blueprint for the Robert Wood Johnson Foundation"
author: "Harindha Fernando – Enterprise Architect, NCINGA"
date: "27 June 2025"
version: "Draft v1.1 – Narrative Edition"
---

> **Purpose of This Document**  
> Provide RWJF leadership with a clear, narrative‑style blueprint describing **why** each architectural decision was taken, **how** it will be implemented, and **what tangible advantage** the Foundation derives at every stage of the solution.

---

## 1 Executive Summary – Why Modernise Now?

Earlier this year Raytion, the connector technology underpinning RWJF’s legacy enterprise search was acquired and officially placed in an end‑of‑life queue.  This single event introduces an unavoidable risk: once vendor support ceases, RWJF’s ability to index Oracle, Adobe AEM and SharePoint content will degrade rapidly, challenging staff productivity and compliance posture.

Against this backdrop, RWJF is already pursuing a strategic shift toward **Salesforce Nonprofit Cloud and Salesforce Data Cloud** for grants management.  In parallel, the IT landscape remains heavily invested in Microsoft 365 and Azure services.  The immediate challenge is therefore two‑fold:

1. **Prevent a search outage** caused by Raytion discontinuation.
2. **Modernise the search experience** to align with RWJF’s evolving Salesforce and Microsoft ecosystems — without duplicating data or sacrificing governance.

NCINGA responds with an **AI‑native, governance‑first architecture** that:

* **Replaces fragile Raytion connectors** with a dual‑layer integration strategy: Denodo for structured virtualisation and UIB secure API gateway for unstructured content delivery.  
* **Aligns to RWJF’s hybrid cloud reality**, offering two validated design paths — Salesforce‑centred or Azure‑centred — while allowing both to co‑exist under a single conversational interface (Sentiyo).  
* **Elevates search to insight**, using semantic understanding, vector retrieval and Retrieval‑Augmented Generation (RAG) so staff receive answers, not just links.  
* **Future‑proofs analytics** via a Databricks lakehouse, ensuring longitudinal grant data and machine‑learning workloads are first‑class citizens, not afterthoughts.

RWJF leadership affirmed the need for a phased approach that protects day‑to‑day knowledge discovery while preparing for Salesforce Data Cloud adoption.  Accordingly, this blueprint focuses on immediate continuity (Raytion exit), medium‑term scalability (governed virtualisation), and long‑term intelligence (AI‑driven experience).

> **Outcome envisioned:** A single, secure, conversational search experience that spans Salesforce records, Microsoft 365 artefacts, AEM collections and Drupal content, backed by policy‑driven governance and actionable analytics.

## 2 Business Drivers & Success Criteria – Why This Matters to RWJF Business Drivers & Success Criteria – *Why This Matters to RWJF*
### 2.1 Mission Alignment
RWJF’s mission is to improve the health and wellbeing of every American.  Faster access to programme knowledge, grant outcomes and policy research directly shortens the feedback loop between insight and impact.

### 2.2 Operational Efficiency
> *Goal:* Reduce time‑to‑information by at least **70 percent** for everyday queries.

*Current pain‑point:* Analysts navigate four separate portals and an Oracle reporting queue before they can assess the success of a grant programme.  By unifying search, analysts retrieve cross‑domain evidence in seconds, freeing time for higher‑order analysis rather than data hunting.

### 2.3 Risk & Compliance
RWJF handles donor information, HR records and occasionally PHI.  The proposed design applies **policy once, enforce everywhere** – ensuring no report, dashboard or AI prompt leaks unauthorised PII.

### 2.4 Cost Optimisation (Addressed as a Next‑Step)
We purposely defer exact ROI numbers until we co‑create a data‑backed TCO baseline with RWJF finance and IT.  The working group and required data points are listed in §9.

---

## 3 Architectural Principles – *How We Keep the Solution Durable*
1. **Governance First** – Access rights checked at query time, not after data is copied.  
2. **Virtualise, Don’t Migrate** – Move data only when analysis demands lake‑scale performance.  
3. **Composable over Monolithic** – Each layer replaceable without vendor lock‑in.  
4. **Observable by Design** – Every request, policy decision and anomaly is logged and surfaced to SOC tooling.  
5. **User‑Centric UX** – The experience is driven by a single conversational interface (Sentiyo) so users never need to understand the plumbing underneath.

---

## 4 Reference Architecture – *What We Are Building and Why*

> **Narrative overview:**  The architecture is intentionally layered.  Each layer fulfils one primary responsibility and exposes a clean contract to the next, ensuring the platform can evolve with minimal disruption.

### 4.1 User Experience Layer – *Sentiyo AI Experience UI*
* **Why?**  Staff deserve Google‑grade search, not Boolean gymnastics.  
* **How?**  Sentiyo accepts plain‑English queries, applies intent detection and streams contextual answers back.  
* **Benefit:**  Faster adoption, lower training overhead and rich conversational clarification when queries are ambiguous.

### 4.2 AI Interpretation Layer – *Sentiyo AI Engine*
* **Why?** Relevance requires semantics, not keywords.  
* **How?**  Queries are embedded into a vector space; similar vectors (documents, rows, snippets) are retrieved, then a Retrieval‑Augmented Generation (RAG) pipeline produces a concise answer with citations.  
* **Benefit:**  Answers are explanatory and trustworthy, linking directly to source evidence.

### 4.3 Policy & Access Control Layer – *Denodo + UIB*
* **Why both?**  
  * *Denodo* virtualises and governs **structured data** – perfect for Salesforce, Workday, SQL – applying row/column masking without ETL delays.  
  * *UIB* (our in‑house API Gateway) secures **unstructured content** – PDFs, AEM pages, SharePoint docs – providing signed URLs and snippet‑level redaction.  
* **Benefit:** One governance model for two very different data shapes, each optimised for its domain.

### 4.4 Data Source Domains
Structured systems remain **in place**; unstructured repositories are accessed **on demand**.  The advantage is two‑fold: business units avoid costly migrations and IT retains authoritative systems of record.

### 4.5 Lakehouse & Vector Store – *Databricks Delta & Vector Storage*
* **Why?**  Some analytics workloads (multi‑year grant impact, ML model training) still need high‑throughput compute.  Databricks provides that engine without forcing every operational query through a lake.  
* **Benefit:**  RWJF gains a future‑proof analytics runway while day‑to‑day queries remain blazing fast through virtualisation.

### 4.6 Visual Analytics – *Power BI*
* **How?**  Dashboards that demand real‑time governance connect live to Denodo.  Heavy trend analysis imports curated tables from Databricks.  
* **Benefit:**  Analysts choose speed **and** depth without maintaining multiple BI tools.

---

## 5 End‑to‑End User Journey & Data Flow – *Experience in Motion*
1. **Authenticate** – Single Sign‑On via Azure AD; user role embedded in JWT.  
2. **Ask** – Sentiyo UI captures a question, e.g., “Which 2023 grants focused on childhood obesity?”  
3. **Interpret** – AI Engine semantically expands the query, generating filter hints (year = 2023, theme = obesity).  
4. **Govern** – Denodo checks whether the user can see grant amounts; UIB confirms the user’s department allows access to programme documents.  
5. **Retrieve** – Structured records stream from Denodo; UIB returns redacted PDF snippets.  
6. **Compose** – Sentiyo stitches these elements into a narrative answer with citations.  
7. **Act** – User drills into a related Power BI dashboard or shares the answer via Teams.

**Advantage:** The user perceives a single, chat‑like experience; the organisation gains enforceable, auditable controls behind the scenes.

---

## 6 Security & Compliance – *Trust by Architecture, Not Afterthought*
* **Data Loss Prevention** – PII classification via Azure Purview propagates to Denodo and UIB, guaranteeing no unmanaged column or file is exposed.  
* **Zero‑Trust Network Segmentation** – All micro‑services run in isolated subnets with egress restricted to known endpoints.  
* **End‑to‑End Encryption** – TLS 1.3 is enforced for internal and external calls.  
* **Audit Certainty** – Every read event (who, what, why) is committed to an immutable log and retained for seven years, aligning with IRS and GDPR retention guidelines.

---

## 7 Deployment & On‑Boarding – *How New Systems Join the Platform*
1. **Profile & Classify** – Data stewards tag sensitivity and ownership.  
2. **Govern** – RBAC and masking rules implemented once in Denodo or UIB.  
3. **Connect** – API / SQL view established; smoke test for policy compliance.  
4. **Index & Embed** – Sentiyo ingests metadata or snippets, updating vectors.  
5. **Validate** – Pilot queries confirm relevance and redaction before go‑live.

**Benefit:** Typical onboarding time drops from eight weeks to under three, freeing IT resources for innovation rather than plumbing.

---

## 8 Operational Excellence & DevSecOps – *Keeping the Lights On, Securely*
* **CI/CD** – GitHub Actions deploy Denodo views, UIB policies, and Sentiyo services via blue/green containers.  
* **Observability** – Logs, traces and metrics funnel into Azure Monitor; anomalies trigger Sentinel playbooks.  
* **Resilience Testing** – Quarterly chaos drills validate fail‑closed behaviour: no policy, no data.

---

## 9 Cost Optimisation & ROI – *Structured Next Step*
This design deliberately postpones hard ROI numbers until RWJF and NCINGA can jointly assemble:

* Legacy licence and infrastructure spend (Oracle, Raytion, bespoke connectors).  
* Productivity baselines (average time‑to‑information, ticket volumes).  
* User satisfaction metrics.  
* Storage and replication overhead.

### Next‑Step Action Plan
*Establish an ROI working group during Phase 1 (Foundation) to produce a board‑ready ROI model within four weeks.*  The deliverable will capture both tangible savings and intangible value (decision velocity, compliance agility).

---

## 10 Road‑Map & Phased Delivery – *Risk‑Managed Transformation*
| Phase | #
