# Enterprise AI Document Processing & Interactive Approval Workflow

## Executive Summary
This project showcases a fully automated, end-to-end document processing and approval system built with **n8n**. It eliminates manual data entry by intercepting incoming emails, extracting critical data using AI, and routing approval requests to management via interactive Telegram buttons. 

This dual-workflow architecture is designed for operations like invoice processing, purchase order approvals, or automated ticketing systems.

## Business Value
* **Zero Manual Data Entry:** AI automatically reads and parses unstructured documents directly from incoming emails.
* **Frictionless Approvals:** Managers can approve or reject requests instantly using Telegram inline buttons—no need to log into complex dashboards.
* **Single Source of Truth:** Google Sheets acts as a live database, instantly reflecting AI extractions and human approval states.
* **Secure Archiving:** Original documents are automatically backed up to Google Drive with strict relational mapping.

## System Architecture & Workflow Logic
This system is divided into two synchronous workflows operating in tandem:

### Workflow 1: AI Ingestion & Data Extraction
1. **Trigger (`Gmail`):** Monitors a dedicated inbox for incoming documents/invoices.
2. **Parallel Processing:**
   * **Storage (`Google Drive`):** Securely uploads the raw attachment to a specific cloud folder.
   * **AI Processing (`Analyze Document`):** Reads the file content and extracts required data points (e.g., amounts, vendor names, dates) using AI.
3. **Data Merging (`Merge`):** Combines the Google Drive file URL with the AI-extracted JSON data.
4. **Database Entry (`Google Sheets`):** Appends the structured payload as a new row in the database, awaiting human review.

### Workflow 2: Interactive Telegram Approvals
1. **Trigger (`Telegram`):** Listens for `callback_query` events (clicks on inline buttons like "Approve" or "Reject").
2. **Routing Logic (`Switch`):** Determines the action based on the button clicked.
   * *Path 0 (Status Check):* Retrieves current row status from Google Sheets and replies to the user.
   * *Path 1 (Execution):* Processes the approval/rejection decision.
3. **State Management (`JavaScript` & `Google Sheets`):** Executes custom logic to sanitize the input and updates the exact target row in the database.
4. **Omnichannel Notification:** 
   * Sends a real-time confirmation message back to the Telegram user.
   * Dispatches an automated Email (`Gmail`) to the original requester with the final decision.

## Tech Stack
* **Orchestration:** n8n (Advanced Workflow Automation)
* **AI Engine:** Intelligent Document Analysis
* **Database & Storage:** Google Sheets API, Google Drive API
* **Communication:** Telegram Bot API (Interactive UI), Gmail API
* **Custom Logic:** JavaScript

---
*Architected to eliminate operational bottlenecks. Open for freelance opportunities and custom automation consulting.*
