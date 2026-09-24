# TS Academy Capstone Project: Automated Feedback & Response Workflows

An end-to-end automated customer feedback management system built using **n8n**, **Airtable**, **Softr**, and **Telegram**.

This project processes customer feedback, calculates sentiment scores, routes critical issues to management, and allows staff to review and send approved responses back to customers seamlessly.

---

## 📁 Repository Structure

* `TS Academy capstone workflow 1.json`: **Workflow 1** – Handles initial customer feedback intake, sentiment analysis/scoring, and routes feedback into Airtable and notification channels.
* `Capstone workflow 2.json`: **Workflow 2** – Manages feedback deep-linking via Telegram, linking user IDs to Airtable records, and alerting managers/HR for negative feedback.
* `Workflow 3_ Send Approved Response.json`: **Workflow 3** – Triggered from the Softr Private Queue dashboard when an employee clicks "Send Message". Fetches the Airtable record, sends the approved response to the customer via Telegram, and updates the draft status to `Sent`.

---

## 🛠️ Tech Stack & Integrations

* **Automation Engine:** [n8n](https://n8n.io/)
* **Database / CRM:** [Airtable](https://airtable.com/)
* **Frontend / Internal Dashboard:** [Softr](https://www.softr.io/)
* **Messaging:** Telegram Bot API (`Group_67_Assistant_bot`)
* **Email Notifications:** Gmail Node

---

## 🚀 Workflows Overview

### Workflow 1: Intake & Routing
1. Captures customer feedback submitted via frontend form.
2. Evaluates sentiment severity levels (`Low`, `Medium`, `High`, `Critical`).
3. Categorizes feedback into `Ready to Post`, `Private Queue`, or `Manager Alerted`.

### Workflow 2: Telegram User Linking & Alerts
1. Links incoming Telegram chat IDs directly to corresponding Airtable feedback records.
2. Sends real-time notifications to team leads and managers when low-sentiment or high-severity feedback is received.

### Workflow 3: Response Dispatch & Status Update
1. Receives a webhook POST request from the Softr Private Queue action button containing the `record_id` and response message.
2. Fetches record details from Airtable.
3. Dispatches the approved AI draft or custom response to the customer via Telegram.
4. Updates the Airtable record `Draft Status` to **`Sent`**.

---

## ⚙️ Setup & Deployment Instructions

1. **Import Workflows into n8n:**
   * Download the `.json` files from this repository.
   * Open n8n $\rightarrow$ **Workflows** $\rightarrow$ **Import from File**.

2. **Configure Credentials:**
   * **Airtable:** Set up OAuth2 API connection to your Airtable base (`Ts Academy feedback capstone`).
   * **Telegram:** Connect your Telegram Bot API token.
   * **Gmail:** Authenticate your Gmail account for outgoing alerts.

3. **Softr Dashboard Webhook Setup:**
   * Set the **Send Message** button on your Private Queue page to trigger a **Custom Workflow** / **Webhook**.
   * Pass the `record_id` in the payload body (`={{ $json.body.record_id }}`).
