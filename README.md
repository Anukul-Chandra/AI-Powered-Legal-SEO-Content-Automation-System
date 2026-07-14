# 🚀 AI-Powered Legal SEO Content Automation System

<p align="center">
  <img src="assets/banner.png" alt="AI-Powered Legal SEO Content Automation System" width="100%">
</p>

## Overview

This n8n automation system generates daily SEO-optimized legal articles for Italian law firm websites. It reads pending topics from a Google Sheet, performs keyword research via SEOZoom, generates full-length Italian legal content using OpenAI (GPT-4.1), runs an AI quality review (GPT-5.1), creates a featured image (GPT-Image-1), and publishes a draft post to WordPress — all without human intervention.

The workflow runs automatically every morning at 08:00 AM Rome time, handles its own topic queue, logs every result to Google Sheets, and sends email notifications on both success and failure.

---

## 🎯 Business Problem

Law firms need consistent, high-quality, SEO-optimized content to rank in local search results. Manual content creation is slow, expensive, and hard to scale. Managing topic queues, keyword research, SEO optimization, internal linking, and WordPress publishing across dozens of articles requires significant editorial overhead.

This system replaces that entire process with a fully automated pipeline — from topic selection through keyword research, AI content generation, quality assurance, image creation, and draft publishing.

---

## ⚡ Core Features

### Automated Topic Processing

The workflow reads pending topics from a Google Sheet (`Studio_Legale_Pasian_Topics`). If no pending topics exist, it automatically recycles previously processed topics by resetting their status back to "Pending," ensuring the system never runs out of content to generate.

### SEOZoom Keyword Research

Each pending topic is sent to the SEOZoom API to fetch related keywords with search volume, keyword difficulty (KD), and keyword opportunity (KO) metrics. The response is parsed and validated before proceeding.

### AI Keyword Selection

A scoring algorithm evaluates every keyword from SEOZoom:

- Filters out keywords with zero search volume
- Scores each keyword using the formula: `(KO × 2) − KD + (volume ÷ 500) + intent_bonus`
- Transactional intent keywords receive a +40 bonus; commercial intent receives +25
- The highest-scored keyword is selected for content generation

### Existing Content Analysis

The system fetches existing published WordPress posts to build an internal linking context. This prevents duplicate content and allows the AI to naturally reference existing articles in new content.

### AI Content Generation

Using OpenAI GPT-4.1 with JSON-structured output, the system generates a complete Italian legal article including:

- H1 title
- 4+ H2 sections
- FAQ section with 3+ Q&A pairs
- SEO metadata (meta title, meta description, slug)
- Image generation prompt
- Internal link suggestions
- Word count targeting 1500+ words

### Content Validation

Every AI-generated article is validated before proceeding:

- Required fields check (title, meta_title, meta_description, slug, article_content, faq)
- Minimum word count enforcement (configurable, default 900)
- Minimum H2 section count (configurable, default 3)
- Minimum FAQ count (configurable, default 3)
- Meta title length validation (max 60 characters)
- Meta description length validation (max 160 characters)

### AI Quality Review

A second OpenAI model (GPT-5.1) acts as a senior legal content editor. It evaluates:

- Content quality and legal accuracy
- SEO optimization (keyword placement, meta tags)
- Structural completeness (H2 sections, FAQ, conclusion, CTA)
- Publication suitability
- Tone and readability for non-expert clients

Articles scoring 6/10 or higher are automatically approved.

### Legal Disclaimer & FAQ Schema Injection

The system appends a standardized legal disclaimer HTML block to every article. It also generates FAQ schema.org JSON-LD markup for rich search results, improving SEO visibility.

### AI Featured Image Generation

Using OpenAI GPT-Image-1, the system generates an infographic-style featured image based on a detailed prompt created during content generation. The prompt includes text overlays, relevant icons, and composition instructions tailored to the legal topic.

### WordPress Draft Publishing

The article is posted to WordPress as a **draft only** — never published automatically. The system sets:

- Title, content, slug, excerpt
- Yoast SEO fields (title, meta description, focus keyword)
- Rank Math SEO fields (title, description, focus keyword)
- Featured image (when available)
- Custom metadata (keyword, word count, quality score, generation date)

A critical safety check verifies the post status is `draft` and throws an error if anything else is returned.

### Google Sheets Logging

Two sheets are maintained:

- **Draft_Logs** — Records every generated draft with date, keyword, title, post ID, edit/preview URLs, word count, quality score, and reviewer notes
- **Studio_Legale_Pasian_Topics** — Tracks which topics have been processed (status updated to "Done" with timestamp)

### Email Notifications

Gmail sends notifications for three scenarios:

- **Success** — Draft ready for review with edit link, quality score, and image status
- **SEOZoom failure** — API unreachable with error details
- **WordPress draft failure** — Post creation error with troubleshooting context

### Error Handling & Resilience

The workflow uses non-blocking error handling on critical external calls (WordPress, SEOZoom, image generation). Failures in image generation or existing post retrieval do not block the pipeline — the system proceeds with graceful fallbacks.

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
      SEOZoom API — Fetch Keyword Metrics
              │
              ├── Failure → Notify Admin (Email)
              │
              ▼
      Parse & Validate SEOZoom Response
              │
              ▼
      Select Best Legal Keyword (Scoring)
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
| SEOZoom API | Keyword research & SEO metrics |
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
| Topic | The topic keyword sent to SEOZoom for research |
| Status | `Pending` → currently in queue; `Done` → processed |
| Last Used | Timestamp when the topic was last processed |

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

**Nodes:** `Get row(s) in sheet` → `Find Pending` → `IF Pending Found?`

The workflow reads all rows from the `Studio_Legale_Pasian_Topics` sheet and searches for the first row where `Status` equals "Pending." If none are found, it reads all "Done" rows and resets their status back to "Pending," creating an infinite recycling loop. This ensures continuous operation without manual topic replenishment.

**Output:** A single pending topic with its row number and metadata.

### Stage 2: Keyword Research & Selection

**Nodes:** `SEOZoom - Fetch Keyword Metrics` → `SEOZoom OK?` → `Code in JavaScript` → `Select Best Legal Keyword1`

The topic is sent to the SEOZoom API to fetch up to 20 related keywords with metrics (search volume, KD, KO, search intent). The response is parsed from JSON and if no valid data is returned, an error notification is sent and the workflow terminates.

A scoring algorithm evaluates each keyword, favoring high-opportunity, low-difficulty keywords with significant search volume. Transactional and commercial intent keywords receive bonus scores.

**Output:** The selected keyword with volume, difficulty, opportunity, intent, and score.

### Stage 3: Content Generation

**Nodes:** `WordPress - Get Existing Posts` → `Prepare Internal Links Context` → `Code in JavaScript1` → `Basic LLM Chain` (with `OpenAI Chat Model`)

Existing published posts are fetched from WordPress. Their titles and links are formatted into a text block that the AI can reference for internal linking suggestions.

The LangChain LLM node sends a detailed prompt to OpenAI GPT-4.1 (with `json_object` response format) that includes the selected keyword, SEO metrics, existing articles context, and strict content requirements. The AI generates a complete Italian legal article as structured JSON.

**Output:** Raw JSON response from OpenAI containing title, meta fields, HTML content, FAQ array, and metadata.

### Stage 4: Content Validation

**Nodes:** `Edit Fields` → `OpenAI Article OK?` → `Parse & Validate Article`

The response is checked for existence. The raw JSON is extracted, stripped of markdown fences, and parsed. Field names are normalized (supporting both `article_html` and `article_content` conventions). If no H1 is provided, it is extracted from the content HTML.

Six validations are performed:

- Required fields must all be present
- Word count must meet the minimum threshold
- H2 section count must meet the minimum threshold
- FAQ count must meet the minimum threshold
- Meta title must not exceed character limit
- Meta description must not exceed character limit

If validation fails, the error is logged to Google Sheets and the admin is notified.

**Output:** A validated, normalized article object with all required fields.

### Stage 5: AI Quality Review

**Nodes:** `OpenAI - AI Quality Review1` (with `OpenAI Chat Model1`) → `Process QA Review Result`

The article is sent to a second OpenAI model (GPT-5.1) acting as a senior legal editor. The review prompt includes the article title, target keyword, word count, H2/FAQ counts, meta data, SEO warnings, and the first 6000 characters of content.

The AI evaluates content quality, SEO optimization, structure, CTA presence, legal tone, and publication readiness. Results are returned as structured JSON.

The `Process QA Review Result` node extracts the review, handles API errors gracefully (auto-approving if the QA call fails), and auto-approves articles with a score of 6/10 or higher.

**Output:** The article enriched with QA metadata (score, issues, improvements, approval status).

### Stage 6: Content Finalization

**Nodes:** `Append Disclaimer + Build Final Content`

A standard Italian legal disclaimer HTML block is appended to the article content (unless one already exists). FAQ schema.org JSON-LD markup is generated from the FAQ array for rich search results. QA reviewer notes are embedded as HTML comments. H2 headings are extracted and counted for final validation.

**Output:** Complete `final_content` with disclaimer, FAQ schema, and QA notes.

### Stage 7: Image Generation

**Nodes:** `Generate an image` → `Image Generated OK?`

The `image_prompt` generated during content creation is sent to OpenAI GPT-Image-1. The prompt specifies an infographic-style image with Italian text overlay, legal icons, and topic-specific visual elements.

If the image is generated successfully (binary data exists), it proceeds to upload. If generation fails, the workflow continues without a featured image.

**Output:** Binary image data (success) or null (fallback).

### Stage 8: Image Upload

**Nodes:** `WordPress - Upload Image` → `Set Image ID - Success` / `Set Image ID - No Image`

The image is uploaded to the WordPress media library via the REST API with Basic Auth. On success, the media ID and URL are extracted. On failure, default null values are set with a skip reason.

**Output:** Article data enriched with `featured_image_id` and `featured_image_url`.

### Stage 9: WordPress Draft Creation

**Nodes:** `Build WordPress Post Body1` → `WordPress - Create Draft` → `Draft Created OK?`

The WordPress post body is constructed with the article title, content, slug (sanitized and transliterated), excerpt, and comprehensive SEO metadata for both Yoast and Rank Math plugins. The post status is hardcoded to `draft` — never `publish`.

The post is sent to WordPress via REST API with Basic Auth. A critical safety check verifies the returned status is `draft` and throws an error if any other status is returned.

**Output:** WordPress post ID, edit URL, preview URL, and draft status.

### Stage 10: Logging & Notification

**Nodes:** `Extract Draft Details` → `Sheets - Log Draft` → `Sheets - Log Used Keyword` → `Send Success Notification`

The draft details are logged to the `Draft_Logs` sheet with all relevant metadata. The topic's status is updated to "Done" with the current timestamp in the `Studio_Legale_Pasian_Topics` sheet.

A success email is sent to the admin containing the article title, keyword, word count, quality score, image status, and a direct link to edit the draft in WordPress.

**Output:** Completed workflow with full audit trail.

---

## 🛡 Validation & Quality Assurance

| Check | Location | Description |
|---|---|---|
| SEOZoom Response Validation | `Code in JavaScript` | Verifies response array exists before processing |
| Zero Volume Keyword Filter | `Select Best Legal Keyword1` | Removes keywords with zero search volume |
| OpenAI Response Existence | `OpenAI Article OK?` | Ensures text content was generated |
| JSON Parse Validation | `Parse & Validate Article` | Strips fences, parses with fallback regex extraction |
| Required Field Validation | `Parse & Validate Article` | Checks title, meta_title, meta_description, slug, article_content, faq |
| Word Count Minimum | `Parse & Validate Article` | Enforces configurable minimum word count |
| H2 Section Count | `Parse & Validate Article` | Enforces minimum H2 sections |
| FAQ Count | `Parse & Validate Article` | Enforces minimum FAQ items |
| Meta Title Length | `Parse & Validate Article` | Enforces maximum character limit |
| Meta Description Length | `Parse & Validate Article` | Enforces maximum character limit |
| AI Quality Review | `Process QA Review Result` | GPT-5.1 evaluates content quality (auto-approve ≥ 6/10) |
| QA API Failure Fallback | `Process QA Review Result` | Auto-approves if QA API is unreachable |
| WordPress Draft Safety Check | `Extract Draft Details` | Verifies status is `draft` — throws error on mismatch |

---

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

---

## 🖼 Image Generation

The system generates a featured image for every article using OpenAI GPT-Image-1. The image prompt is created during content generation and specifies:

- A bold short Italian headline text overlay (2–5 words)
- 2–3 relevant icons/symbols (scales of justice, gavel, legal documents, plus topic-specific icons)
- Infographic-style layout
- Brand colors and professional aesthetic

If image generation succeeds, it is uploaded to WordPress and set as the post's featured image. If generation or upload fails, the draft is created without an image and the admin is notified in the success email.

---

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
- [x] SEO keyword research via SEOZoom
- [x] AI-powered keyword scoring and selection
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

---

## 📂 Repository Structure

```
AI-Powered-Legal-SEO-Content-Automation-System/
│
├── README.md
├── AI-Powered_Legal_SEO_Content_Automation_System_.json
│
├── docs/
│   └── configuration-guide.md
│
└── assets/
    └── banner.png
```

---

## 🚀 Installation

### Requirements

- n8n instance (self-hosted or n8n.cloud)
- Active API keys for: OpenAI, SEOZoom, Google Sheets, Gmail
- WordPress website with REST API enabled
- Google Cloud project with Sheets API enabled

### Setup

1. **Configure credentials in n8n:**

   | Credential Name | Type | Required Scopes |
   |---|---|---|
   | OpenAI API | OpenAI API | Model access (gpt-4.1, gpt-5.1, gpt-image-1) |
   | SEOZoom | Generic API Key | Keyword research |
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

   - SEOZoom endpoint, country, language, limit
   - WordPress site URL
   - Content parameters (language, word count, H2/FAQ minimums)
   - Firm identity (name, city, phone, email, practice areas)
   - Admin/from email addresses
   - Google Sheet ID
   - Retry settings

4. **Update credential placeholders:**

   - Replace `YOUR_SEOZOOM_API_KEY` in the `SEOZoom - Fetch Keyword Metrics` node
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
| Google Sheets | OAuth2 | Topic queue management, draft logging |
| WordPress | HTTP Basic Auth | Draft creation, media upload |
| Gmail | OAuth2 | Success/failure notifications |

---

## 🔮 Future Improvements

- **Automatic publishing schedule** — Optional flag to auto-publish drafts after a configurable review period
- **Multi-language support** — Extend beyond Italian to additional jurisdictions
- **Competitor content analysis** — SEOZoom competitive data to identify content gaps
- **Slack/Teams notifications** — Additional notification channels beyond email
- **Analytics dashboard** — Track content performance metrics post-publication
- **Batch processing** — Process multiple pending topics per trigger instead of one
- **Content calendar integration** — Sync with Google Calendar for scheduled publishing dates
- **Category and tag assignment** — Auto-categorize posts based on practice area mapping

---

## 📸 Workflow Screenshots

<details>
<summary>Complete Workflow</summary>
<br>
<img src="assets/screenshots/complete-workflow.png" alt="Complete Workflow" width="800"/>
</details>

<details>
<summary>Google Sheets - Topic Queue</summary>
<br>
<img src="assets/screenshots/google-sheets-topics.png" alt="Google Sheets Topic Queue" width="800"/>
</details>

<details>
<summary>Google Sheets - Draft Logs</summary>
<br>
<img src="assets/screenshots/google-sheets-drafts.png" alt="Google Sheets Draft Logs" width="800"/>
</details>

<details>
<summary>WordPress Draft</summary>
<br>
<img src="assets/screenshots/wordpress-draft.png" alt="WordPress Draft" width="800"/>
</details>

<details>
<summary>AI Quality Review</summary>
<br>
<img src="assets/screenshots/ai-quality-review.png" alt="AI Quality Review Output" width="800"/>
</details>

---

## 👨‍💻 Author

**Anukul Chandra**

- AI Automation Engineer
- Workflow Automation Specialist
- AI Solutions Developer

<p align="center">
  Built with n8n · OpenAI · SEOZoom · WordPress
</p>
