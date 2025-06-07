# ✨ Airflow DAG Sandbox – Vision & Proposal

## Overview

The **Airflow DAG Sandbox** is a proposed developer-focused enhancement to the Airflow UI that allows authorized users to **experiment with alternate DAG code paths safely, quickly, and securely** — without altering production DAGs or going through the full Git-based promotion workflow.

This feature aims to improve **developer velocity and debugging capability** while maintaining **strong security boundaries and auditability**.

---

## 🎯 Vision

> “Empower Airflow users with a secure, disposable sandbox to quickly test alternate DAG logic — without sacrificing safety, auditability, or engineering best practices.”

Today, any changes to DAGs — even small experiments — must be committed via Git and deployed through the production DAG parsing pipeline. This is safe, but slow. The DAG Sandbox offers a middle ground for **non-persistent, in-memory testing** of DAG modifications through a self-contained, isolated execution environment.

---

## 🚀 Objectives

- Allow users to **edit and test existing DAGs** via the Airflow UI in a temporary sandbox.
- Provide **manual-only** execution to avoid unintended scheduling.
- Prevent any changes from being persisted or affecting production DAGs.
- Enable **in-memory parsing and execution** of modified DAGs.
- Ensure strict **isolation from Airflow connections, secrets, variables, and configs**.
- Capture and log **audit metadata** for each sandbox session and run.
- Require **user authentication** before enabling sandbox access.
- Design with **developer productivity** and **security-first principles** in mind.

---

## ✅ Supported Use Cases

| Use Case                | Description                                                                 |
|-------------------------|-----------------------------------------------------------------------------|
| 🔧 Debugging DAG logic  | Identify and fix issues like task failure, dependency misconfigurations, or param passing errors |
| 🧪 Testing alternate paths | Test alternate task branches, operator changes, or parameter tweaks         |
| 🛡️ Safe experimentation | Trial DAG ideas without risk of impacting production DAGs or metadata       |
| 🔍 Execution visibility  | Get full debug logs, run traces, and impact metadata for each test          |
| 🚫 Dry-run feature testing | Run DAGs without accessing real variables, connections, or secrets         |

---

## 🔒 Design Principles

- **Isolation by Default**: Sandbox DAGs are parsed and run in-memory, never written to disk or registered with the scheduler.
- **Manual Execution Only**: No scheduling, triggering, or task retries — every execution must be explicitly invoked.
- **Config Segregation**: Sandbox loads a minimal, restricted config without access to production connections or variables.
- **User Verification**: Only authorized users (via GitHub OAuth or internal auth) may use the feature.
- **Audit & Traceability**: All sandbox sessions are logged with user identity, code changes, execution results, and service calls.

---

## 🧩 High-Level Architecture
```
[ UI: DAG Details Page ]
        │
"Open in Sandbox" Button
        │
        ▼
[ Code Editor Modal (React) ]
        │
  Manual Trigger Request
        │
        ▼
[ Sandbox API Endpoint (FastAPI) ]
        │
Parse + Execute DAG in-memory (isolated context)
        │
        ▼
[ Results + Logs + Audit Metadata Returned ]
```

**Key Characteristics:**

- DAGs are never saved to disk or registered in the DagBag.
- Task execution is performed in a contained subprocess or container.
- Logs and metadata are streamed back to the UI for review.
- Sandbox runs are ephemeral and auto-expire.
- No access to Airflow secrets, environment variables, or external services unless explicitly configured by the user in the sandbox.

---

## 🔐 Security & Safety Controls

- ✅ No persistence of edited code
- ✅ DAGs excluded from scheduler & auto-parsing
- ✅ No access to Airflow secrets or connections
- ✅ Authenticated and rate-limited access only
- ✅ Full audit logging and observability per sandbox session
- ✅ Enforced runtime and resource limits

---

## 🏗️ Status

This is currently a **proof-of-concept** under development in a personal fork of Apache Airflow. Contributions, feedback, and collaboration are welcome to evolve this into a production-ready feature and potential AIP submission.

---

## 📬 Feedback & Contributions

We’d love to hear your thoughts on:

- Use cases we may have missed
- UI/UX suggestions for the sandbox editor
- Concerns around multi-user usage or scheduling behavior
- Ideas to enhance security and observability

---

## 📄 License

This proposal and accompanying code follow the Apache Airflow licensing model and are intended to be contributed under the Apache License 2.0.
