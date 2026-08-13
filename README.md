# 🚀 AI-Powered Legal SEO Content Automation System

<p align="center">
  <img src="https://img.shields.io/badge/n8n-%23000000.svg?style=for-the-badge&logo=n8n&logoColor=green" alt="n8n"/>
  <img src="https://img.shields.io/badge/OpenAI%20GPT--4.1-%23412991.svg?style=for-the-badge&logo=openai&logoColor=white" alt="OpenAI GPT-4.1"/>
  <img src="https://img.shields.io/badge/OpenAI%20GPT--5.1-%23412991.svg?style=for-the-badge&logo=openai&logoColor=white" alt="OpenAI GPT-5.1"/>
  <img src="https://img.shields.io/badge/GPT%20Image%201-%23412991.svg?style=for-the-badge&logo=openai&logoColor=white" alt="GPT Image 1"/>
  <img src="https://img.shields.io/badge/SEOZoom%20API-%23FF6F00.svg?style=for-the-badge&logo=data:image/svg%2bxml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSI2NCIgaGVpZ2h0PSI2NCIgZmlsbD0ibm9uZSI+PHBhdGggZD0iTTMyIDBjMTcuNjczIDAgMzIgMTQuMzI3IDMyIDMycy0xNC4zMjcgMzItMzIgMzJTMCA0OS42NzMgMCAzMiAxNC4zMjcgMCAzMiAweiIgZmlsbD0iI2ZmZiIvPjwvc3ZnPg==&logoColor=white" alt="SEOZoom API"/>
  <img src="https://img.shields.io/badge/Skyvern-%236366F1.svg?style=for-the-badge&logo=data:image/svg%2bxml;base64,PHN2ZyB3aWR0aD0iMzIiIGhlaWdodD0iMzIiIHZpZXdCb3g9IjAgMCAzMiAzMiIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj48cmVjdCB3aWR0aD0iMzIiIGhlaWdodD0iMzIiIHJ4PSI2IiBmaWxsPSIjNjM2NkYxIi8+PHRleHQgeD0iOCIgeT0iMjMiIGZvbnQtZmFtaWx5PSJzeXN0ZW0tdWksIHNhbnMtc2VyaWYiIGZvbnQtc2l6ZT0iMTgiIGZvbnQtd2VpZ2h0PSI3MDAiIGZpbGw9IndoaXRlIj5TPC90ZXh0Pjwvc3ZnPg==&logoColor=white" alt="Skyvern"/>
  <img src="https://img.shields.io/badge/WordPress-%2321759B.svg?style=for-the-badge&logo=wordpress&logoColor=white" alt="WordPress"/>
  <img src="https://img.shields.io/badge/Google%20Sheets-%2334A853.svg?style=for-the-badge&logo=googlesheets&logoColor=white" alt="Google Sheets"/>
  <img src="https://img.shields.io/badge/Gmail-%23EA4335.svg?style=for-the-badge&logo=gmail&logoColor=white" alt="Gmail"/>
  <img src="https://img.shields.io/badge/JavaScript-%23F7DF1E.svg?style=for-the-badge&logo=javascript&logoColor=black" alt="JavaScript"/>
  <img src="https://img.shields.io/badge/REST%20API-%23000000.svg?style=for-the-badge&logo=fastapi&logoColor=white" alt="REST API"/>
  <img src="https://img.shields.io/badge/AI%20Automation-%2300C853.svg?style=for-the-badge&logo=robotframework&logoColor=white" alt="AI Automation"/>
</p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/54463288-2c6a-4d6d-9d2e-6d6c4147ae69" alt="Banner" width="100%">
</p>

## Overview

An Italian law firm needed to publish one SEO-optimised legal article every day on their website. Their team was managing everything manually — researching legal keywords through multiple tools, writing long-form content, creating featured images, formatting WordPress posts, and reviewing each article before publication. They wanted a system that could handle the entire production pipeline automatically while still keeping a lawyer in the loop for final review.

This project delivers exactly that. It is an end-to-end n8n automation workflow that reads content topics from a Google Sheet, researches and selects the best legal keyword through browser-based keyword research automated by Skyvern, generates a complete Italian legal article using OpenAI GPT-4.1, runs an AI quality review with GPT-5.1, creates a featured image with GPT-Image-1, and posts a draft to WordPress — all on a daily schedule with zero manual effort. Every step is logged, and the system sends email notifications on both success and failure.

---

## 🎯 Business Problem

The client wanted to publish one high-quality legal article every day, but the entire process was manual. Each article required finding SEO opportunities, researching keyword metrics, analysing search intent, planning article structure, writing long-form legal content, crafting SEO metadata, creating a featured image, uploading everything into WordPress, and reviewing the draft before it could go live.

This workflow consumed hours per article, required switching between half a dozen platforms, and made it difficult to maintain a consistent daily publishing schedule. The client needed a reliable automated system that could handle the heavy lifting of research, writing, image creation, and WordPress preparation — while still allowing their legal team to review every article before it was published. This project was built to solve exactly those business challenges through intelligent workflow automation.

---

## ⚡ Core Features

### ➡️ Automated Topic Processing
Manages a self-replenishing content queue via Google Sheets. Topics marked as `Pending` are processed in sequence; once all topics are completed, the queue automatically recycles to ensure uninterrupted operation without manual intervention.

### ➡️ Skyvern Browser-Based Keyword Research
Dispatches a Skyvern browser-automation run that researches the current topic in the SEO research web interface while excluding the previously used keywords, then polls the run until it completes and extracts the keyword candidates for processing.

### ➡️ AI Keyword Selection
Processes the extracted keyword results in n8n — parsing and normalizing Italian-formatted metrics, removing duplicates, and ranking candidates by search volume to select the primary keyword plus the top related terms for the article.

### ➡️ Existing Content Analysis
Pulls the WordPress post history to build an internal linking context and to derive the previously used keywords that are excluded from the current research round, avoiding duplication and overlapping topics.

### ➡️ AI Content Generation
Leverages OpenAI GPT-4.1 to produce a complete Italian legal article in a single pass — including H1 title, structured H2 sections, FAQ content, SEO metadata, an image prompt, and internal link suggestions — all returned as structured JSON.

### ➡️ Content Validation
Every generated article is checked against configurable thresholds for word count, H2 section count, FAQ count, meta title length, and meta description length. Invalid articles are logged and flagged for review.

### ➡️ AI Quality Review
A second model (GPT-5.1) performs a senior-editor-level audit of content quality, legal accuracy, SEO optimisation, and structural completeness. Drafts are auto-approved after review, and the quality score with reviewer notes is recorded in the audit log.

### ➡️ Humanization & Italian Language Enforcement
A dedicated LLM pass rewrites the article into a natural human tone, and a context-aware fixer detects and corrects any residual non-Italian wording. If a fix cannot be fully applied, the admin is notified and the pipeline continues.

### ➡️ Legal Disclaimer & Schema Injection
Appends a standardised Italian legal disclaimer to every article and injects FAQ schema.org JSON-LD markup to enhance search-result visibility.

### ➡️ AI Featured Image Generation
Generates an infographic-style featured image via GPT-Image-1, incorporating topic-specific legal icons, text overlays, and brand-consistent composition.

### ➡️ WordPress Draft Publishing
Creates a draft post (never published automatically) with complete Yoast and Rank Math SEO metadata, slug, excerpt, optional featured image, and internal review annotations. A built-in safety check verifies the draft status before proceeding.

### ➡️ Audit Logging
Records every generated draft in a Google Sheet (keyword, title, word count, quality score, edit link) and updates the topic queue with a completion timestamp, providing a full audit trail.

### ➡️ Email Notifications
Sends targeted Gmail alerts for successful draft creation (with edit link) as well as failures in article generation, language validation, image generation, and WordPress draft creation — each with relevant context for rapid resolution.

### ➡️ Error Resilience
Non-blocking error handling across all external integrations. Failures in image generation, content retrieval, or API calls do not halt the pipeline; the system degrades gracefully and continues processing.

---

## 🌐 Skyvern Browser-Based Keyword Research

Keyword research is the one stage of the pipeline that could not be handled reliably by the API alone.

**Previous approach.** Keyword research was primarily handled through the SEOZoom API. The API-based approach was useful, but for some topics it was not consistently returning enough usable keywords for the workflow's needs.

**Problem.** A human manually searching through the SEO research web interface could get better results by entering the current topic while excluding the previously used keywords. That browser-based approach produced sufficient keyword results where the API response fell short.

**Solution.** Skyvern was introduced as the browser automation layer to reproduce that manual research process automatically. It complements the existing n8n + SEOZoom workflow by handling the browser-based research step, while n8n continues to handle orchestration, keyword processing, AI generation, validation, WordPress publishing, and logging.

```
Current topic + excluded keywords
            ↓
Skyvern browser automation
            ↓
Web-based keyword research
            ↓
Extracted keyword results
            ↓
n8n keyword processing/selection
            ↓
Content generation pipeline
```

---

## 🏗 System Architecture

```
Schedule Trigger (08:00 Rome)
      │
      ▼
Workflow Config
      │
      ▼
Google Sheets — Read Topics
      │
      ▼
Find Pending Topic
      │
      ├── No Pending Found?
      │       │
      │       ▼
      │   Google Sheets — Read Done Topics
      │       │
      │       ▼
      │   Reset Done → Pending (loop)
      │
       ├── Pending Found?
               │
               ▼
       WordPress — Get Post History (Used Keywords)
               │
               ▼
       Extract Primary & Excluded Keywords
               │
               ▼
       Skyvern — Browser Keyword Research
               │
               ├── Running/Canceled → Poll / Re-run
               │
               ▼
       Parse & Select Best Legal Keyword
               │
               ▼
       WordPress — Get Existing Posts
              │
              ▼
      Prepare Internal Links Context
              │
              ▼
      OpenAI GPT-4.1 — Generate Article (JSON)
              │
              ├── Failure → Log Error + Notify Admin
              │
              ▼
      Parse & Validate Article
              │
              ▼
       OpenAI GPT-5.1 — AI Quality Review
               │
               ▼
       Process QA Review Result
               │
               ▼
       Humanize Content + Fix Residual English
               │
               ▼
       Append Disclaimer + FAQ Schema
              │
              ▼
      OpenAI GPT-Image-1 — Generate Featured Image
              │
              ├── Success → Upload to WordPress Media
              │   │
              │   └── Set Image ID (Success)
              │
              └── Failure → Set Image ID (No Image)
                      │
                      ▼
              Build WordPress Post Body
                      │
                      ▼
              WordPress — Create Draft (POST)
                      │
                      ├── Failure → Notify Admin
                      │
                      ▼
              Verify Draft Status (Safety Check)
                      │
                      ▼
              Google Sheets — Log Draft
                      │
                      ▼
              Google Sheets — Mark Topic Done
                      │
                      ▼
              Gmail — Send Success Notification
```

---

## 🛠 Tech Stack & Integrations

| Technology | Purpose |
|---|---|
| n8n | Workflow automation & orchestration |
| OpenAI GPT-4.1 | Article content generation |
| OpenAI GPT-5.1 | AI quality review & evaluation |
| OpenAI GPT-Image-1 | Featured image generation |
| SEOZoom | SEO keyword & metrics data source (researched via browser automation) |
| Skyvern | AI browser automation for web-based keyword research |
| Google Sheets | Topic queue management & draft logging |
| WordPress REST API | Draft creation & media upload |
| Gmail | Email notifications (success/failure) |
| LangChain | LLM chain management within n8n |
| JavaScript | Custom logic, data transformation, validation |

---

## 📊 Google Sheets Structure

### Sheet 1: `Studio_Legale_Pasian_Topics`

The topic queue. Each row represents a content topic to process.

| Column | Description |
|---|---|
| Topic | The topic keyword that drives keyword research |
| Status | `Pending` → currently in queue; `Done` → processed |
| Last Used | Timestamp when the topic was last processed |
| Used Keyword | The keyword used for the most recent article on this topic |

If all topics are "Done," the workflow resets them to "Pending" to recycle the queue.

### Sheet 2: `Draft_Logs`

An audit log of every generated draft.

| Column | Description |
|---|---|
| Date | Timestamp in Rome timezone (dd/MM/yyyy HH:mm:ss) |
| Keyword | The SEO keyword selected for the article |
| Post Title | The generated article title |
| Post ID | WordPress post ID of the draft |
| Draft Edit URL | Direct link to edit the draft in WordPress admin |
| Draft Preview URL | Preview link for the draft |
| Status | WordPress post status (always "draft") |
| Word Count | Total word count of the generated article |
| Quality Score | AI quality review score (out of 10) |
| Image Uploaded | Whether the featured image was uploaded (true/false) |
| Reviewer Notes | Notes from the AI quality review |

---

## 🔄 Workflow Pipeline

### Stage 1: Topic Queue Management

- Reads the Google Sheet topic queue for the next `Pending` item
- Automatically recycles completed topics when the queue is empty, ensuring the pipeline never runs dry
- Each topic flows through the system one at a time per scheduled run

### Stage 2: Keyword Research & Selection

- Extracts the previously used keywords from the published post history to build the exclusion list
- Dispatches a Skyvern browser-automation run that researches the current topic in the web interface while excluding already-used keywords, then polls the run until it completes
- Parses and normalizes the extracted keyword results, deduplicates them, and ranks candidates by search volume to select the primary keyword
- The top keyword advances to content generation

### Stage 3: AI Content Generation

- Fetches existing WordPress posts for internal linking context
- Sends the selected keyword, SEO data, and existing articles to GPT-4.1
- Generates a complete Italian legal article with title, HTML content, FAQ section, SEO metadata, and image prompt

### Stage 4: Validation & Quality Review

- Validates the generated article against strict requirements: word count, H2 count, FAQ count, and SEO field lengths
- Passes the article to GPT-5.1 for a senior-editor-level quality assessment
- Drafts are auto-approved after review, with the quality score and reviewer notes logged in the audit trail
- Runs a dedicated humanization pass, then detects and fixes any residual non-Italian wording before finalization

### Stage 5: Content Finalization

- Appends a standardized Italian legal disclaimer to the article
- Injects FAQ schema.org JSON-LD markup for rich search results
- Prepares the complete HTML payload for WordPress

### Stage 6: Image Generation & Upload

- Generates an infographic-style featured image via GPT-Image-1 with legal-themed visuals and text overlay
- Uploads the image to the WordPress media library
- If generation or upload fails, the pipeline continues without a featured image

### Stage 7: WordPress Draft Creation

- Builds the WordPress post with full Yoast and Rank Math SEO metadata
- Creates a draft post via the WordPress REST API (status is always `draft` — never published automatically)
- Verifies the draft status as a safety check before proceeding

### Stage 8: Logging & Notification

- Logs the draft details (keyword, title, word count, quality score, edit link) to Google Sheets
- Marks the topic as `Done` in the queue with a timestamp
- Sends a success email to the admin with the draft preview link and key metrics



## 🤖 AI Components

### GPT-4.1 — Content Generation

**Model:** `gpt-4.1` with `json_object` response format

The primary content generation model receives a detailed prompt containing the target keyword, SEO metrics, existing articles for internal linking, and strict formatting requirements. It returns a complete article as structured JSON with fields for title, HTML content, FAQ, metadata, and image generation prompt.

The prompt enforces:
- Italian language only
- Professional but accessible legal tone
- 1500+ word minimum body text
- 4+ H2 sections
- FAQ section with 3+ Q&A pairs
- Call to Action paragraph
- Natural law firm references tied to the topic
- Internal link suggestions from existing content

### GPT-5.1 — Quality Review

**Model:** `gpt-5.1` with `json_object` response format

Acts as a senior legal content editor. Reviews content quality, SEO optimization, structural completeness, legal accuracy, and publication readiness. Returns a structured evaluation with quality score, approval flag, and improvement suggestions.

### GPT-Image-1 — Featured Image

**Model:** `gpt-image-1`

Generates infographic-style legal images based on AI-generated prompts. The prompt includes text overlays, legal icons (scales of justice, gavel, documents), topic-specific visual elements, and composition instructions.



## 🌐 WordPress Automation

- **Draft Creation:** Posts are created via `wp/v2/posts` endpoint with status `draft`. Automatic publishing is intentionally disabled.
- **Metadata:** Yoast SEO and Rank Math SEO fields are populated (title, meta description, focus keyword).
- **Slug Generation:** Slugs are sanitized, transliterated (accents removed), lowercased, and truncated to 80 characters.
- **Featured Image:** Attached to the post via `featured_media` field when available.
- **Safety Check:** The workflow verifies the returned post status is `draft` and raises an error if any other status is detected.
- **Custom Fields:** Internal review metadata is stored as custom fields (keyword, word count, quality score, generation date).

---

## 📈 Business Impact

### Automated Tasks

- [x] Topic queue management (read, process, recycle)
- [x] Browser-based SEO keyword research (Skyvern)
- [x] Keyword scoring and selection
- [x] Humanization and Italian language enforcement
- [x] Existing content analysis for internal linking
- [x] Full-length Italian legal article generation
- [x] Content validation (structure, length, SEO)
- [x] AI quality review with scoring
- [x] Legal disclaimer and FAQ schema injection
- [x] AI featured image generation
- [x] WordPress media upload
- [x] WordPress draft creation with SEO metadata
- [x] Google Sheets audit logging
- [x] Email notifications (success and failure)
- [x] Error handling with graceful fallbacks

### Benefits

- **Zero manual effort** — from topic to draft with no human intervention
- **Consistent quality** — AI review ensures every article meets editorial standards
- **Always fresh content** — topic queue automatically recycles when exhausted
- **SEO-optimized** — keyword research, meta fields, FAQ schema, internal linking
- **Full audit trail** — every draft is logged with quality scores and metadata
- **Safe by design** — articles are always drafts, never published automatically
- **Resilient** — individual failures (image, API, network) do not block the pipeline



## 🚀 Installation

### Requirements

- n8n instance (self-hosted or n8n.cloud)
- Active API keys for: OpenAI, SEOZoom, Skyvern, Google Sheets, Gmail
- Skyvern instance (self-hosted via Docker, or Skyvern Cloud) with API access
- WordPress website with REST API enabled
- Google Cloud project with Sheets API enabled

### Setup

1. **Configure credentials in n8n:**

   | Credential Name | Type | Required Scopes |
   |---|---|---|
   | OpenAI API | OpenAI API | Model access (gpt-4.1, gpt-5.1, gpt-image-1) |
   | SEOZoom | Generic API Key | Keyword research |
   | Skyvern | Skyvern API Key | Browser-based keyword research |
   | Google Sheets | OAuth2 | Sheets read/write |
   | WordPress | HTTP Basic Auth | Posts + Media REST API |
   | Gmail | OAuth2 | Send email notifications |

2. **Prepare the Google Sheet:**

   Create a spreadsheet named `Studio_Legale_Pasian_Topics` with two sheets:

   **Sheet 1 (Studio_Legale_Pasian_Topics):**

   | Topic | Status | Last Used |
   |---|---|---|
   | divorzio consensuale | Pending | |
   | separazione legale | Pending | |
   | ... | Pending | |

   **Sheet 2 (Draft_Logs):**

   | Date | Keyword | Post Title | Post ID | Draft Edit URL | Draft Preview URL | Status | Word Count | Quality Score | Image Uploaded | Reviewer Notes |
   |---|---|---|---|---|---|---|---|---|---|---|

3. **Configure the `Workflow Config` node:**

   Edit the JavaScript configuration object with your specific values:

   - Skyvern workflow ID, API key, and API base URL used for polling run status
   - WordPress site URL
   - Content parameters (language, word count, H2/FAQ minimums)
   - Firm identity (name, city, phone, email, practice areas)
   - Admin/from email addresses
   - Google Sheet ID
   - Retry settings

4. **Update credential placeholders:**

   - Replace `YOUR_WORKFLOW_ID` in the Skyvern keyword research node (the sub-workflow to dispatch)
   - Replace `YOUR_API_KEY_HERE` in the `Skyvern Get Run` node
   - Replace `Basic WORDPRESS_BASE64_CREDENTIALS` in the WordPress HTTP nodes
   - Replace `YOUR_GOOGLE_SHEET_ID` in all Google Sheets nodes
   - Replace `your-notification-email@example.com` in all Gmail nodes

5. **Import the workflow:**

   - Open your n8n instance
   - Go to **Workflows** → **Import from File**
   - Select `AI-Powered_Legal_SEO_Content_Automation_System_.json`
   - Activate the workflow

---

## 🔐 Required Credentials

| Service | Credential Type | Purpose |
|---|---|---|
| OpenAI | API Key | Content generation, quality review, image generation |
| SEOZoom | API Key (query parameter) | Keyword research |
| Skyvern | API Key | Browser-based keyword research |
| Google Sheets | OAuth2 | Topic queue management, draft logging |
| WordPress | HTTP Basic Auth | Draft creation, media upload |
| Gmail | OAuth2 | Success/failure notifications |



## 👨‍💻 Author

**Anukul Chandra**

- AI Automation Engineer
- Workflow Automation Specialist
- AI Solutions Developer

<p align="center">
  Built with n8n · OpenAI · Skyvern · SEOZoom · WordPress
</p>
