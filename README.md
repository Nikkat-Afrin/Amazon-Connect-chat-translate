# Real-Time Multilingual Chat Translation on AWS Connect ☁️🌐

**A serverless web app that adds real-time, two-way language translation to Amazon Connect chat — a customer chats in their own language while the agent works in English, with automatic language detection.**

![AWS](https://img.shields.io/badge/AWS-Connect%20%7C%20Translate%20%7C%20Comprehend%20%7C%20Lambda%20%7C%20Amplify-orange) ![Frontend](https://img.shields.io/badge/Frontend-React-blue) ![Type](https://img.shields.io/badge/Type-Fork%20%E2%80%94%20Deployed%20%26%20Configured-yellow)

---

> ### ⚠️ Attribution (please read first)
> **This is a fork of an open-source AWS sample**, originally created by **VoiceFoundry / AWS** (contributors: Daniel Bloy, Bob Strahan, Vishal Nayak, EJ Ferrell, Kishore Dhamodaran — see [`UPSTREAM_README.md`](UPSTREAM_README.md) and [`LICENSE`](LICENSE)). I did **not** write the original application code. My contribution was to **deploy, configure, test, and document** the solution as a Cloud Computing course project, and to write the architecture/operations notes below. The original repo: *Amazon Connect Chat Translate (VoiceFoundry)*. This fork is published for portfolio/learning purposes with full credit to the original authors.

---

## 🧭 What the system does
On an Amazon Connect chat, a customer can type in (for example) French. The agent's web app:
1. **Detects** the customer's language automatically with **Amazon Comprehend** (or honors a preset `x_lang` contact attribute),
2. **Translates** the customer's messages to English with **Amazon Translate** so the agent reads them natively,
3. **Translates the agent's English replies back** into the customer's language before sending.

It supports multiple concurrent chats in different languages and optional **Custom Terminology** for brand/domain-specific phrases.

## 🏗️ Architecture

![Architecture](artifacts/Arch.png)

```
React app (Amplify-hosted)
      │  embeds the Amazon Connect Contact Control Panel (CCP) as an iFrame
      ▼
Amazon Connect (chat)  ──►  Amazon Comprehend  (language detection)
      │
      ▼
API Gateway  ──►  AWS Lambda  ──►  Amazon Translate  (+ Custom Terminology)
      ▲
      └──  serverless, pay-per-use; deployed & hosted via AWS Amplify
```

**AWS services:** Amazon Connect · Amazon Translate · Amazon Comprehend · AWS Lambda · Amazon API Gateway · AWS Amplify (hosting + CI/CD).

## 🙋 What I did (my contribution)
- **Deployed** the serverless stack end-to-end via the **AWS Amplify** console (no-CLI flow), connecting the GitHub repo and provisioning the backend (Lambda functions, predictions for Translate/Comprehend, auth, hosting).
- **Configured** the Amazon Connect instance: set `REACT_APP_CONNECT_REGION` / instance-URL environment variables, and added the Amplify hosting URL to the Connect **Approved Origins** so the CCP embeds correctly.
- **Tested** the end-to-end flow with a live customer↔agent chat across languages (e.g., French ↔ English) and verified automatic language detection.
- **Set up Custom Terminology** (the `connectChatTranslate` CSV) and validated translations.
- **Authored** the architecture overview, cost analysis, and operations notes (this README) and the course write-up.

## 💸 Cost (serverless, pay-per-use)
All services fall under the **AWS Free Tier** for light use. Indicative per-message/usage pricing: Amazon Translate ($15 / M chars), API Gateway ($1 / M requests), Lambda ($0.20 / M requests), Amplify build/host (cents). See [`UPSTREAM_README.md`](UPSTREAM_README.md) for the detailed cost table.

## ▶️ Deploy it yourself (summary)
1. Fork the repo and connect it in the **AWS Amplify** console (Amplify Hosting → GitHub).
2. Add environment variables: `REACT_APP_CONNECT_REGION`, `REACT_APP_CONNECT_INSTANCE_URL` (and set `src/.env`'s `REACT_APP_CCP_URL` to *your* Connect instance — the committed value is a redacted placeholder).
3. After deploy (~10 min), add the Amplify URL to Amazon Connect **Approved Origins**.
4. Start a test chat and confirm translation + language detection. Full steps in [`UPSTREAM_README.md`](UPSTREAM_README.md).

> **Note:** running this requires *your own* Amazon Connect instance and AWS account; it is not a standalone local app.

## 🔐 Security note
`src/.env` originally contained a specific Connect instance URL (the original authors' demo instance); it has been **replaced with a placeholder** in this fork. Provide your own instance URL at deploy time.

## 🛠️ Tech stack
`AWS Amplify` · `Amazon Connect` · `Amazon Translate` · `Amazon Comprehend` · `AWS Lambda` · `API Gateway` · `React` · `JavaScript` · `CloudFormation (Amplify-generated)`

---
*Cloud Computing course project. Fork of the VoiceFoundry/AWS open-source Amazon Connect Chat Translate sample — all original application code credited to its authors under the included LICENSE.*
