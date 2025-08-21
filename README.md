# 🕸️ Web Scrap Agent – AI Job Posting Pipeline

This n8n workflow automates the full pipeline of scraping job news from various websites, parsing the relevant information, rewriting it with AI, and publishing it as a structured WordPress post — including the featured image.

---

## 🚀 Features

- ⏱ **Every 2 minutes**: Automatically triggered scraping.
- 🌐 **Multi-source parsing**:
  - `wadhefa.com/news.php`
  - `ewdifh.com`
  - `wdeftksa.com`
- 🤖 **AI-Powered Rewriting**:
  - Uses OpenAI to rewrite job posts into Arabic.
  - Original, SEO-optimized, structured output.
- 🏷️ **Automatic Categorization**:
  - Sector, City, Type (full-time, etc.)
- 🖼️ **Image Handling**:
  - Downloads job image/logo.
  - Uploads it to WordPress.
- 📝 **WordPress Integration**:
  - Creates a new post with title, content, featured image.

---

## 🔁 Flow Overview

```mermaid
graph TD
  A[Schedule Trigger (every 2 mins)] --> B[Fetch HTML from sources]
  B --> C[Extract Latest Job Post (Regex)]
  C --> D[Send to AI Agent]
  D --> E[AI reads job link, rewrites job post in Arabic]
  E --> F[Extract structured JSON output]
  F --> G[Download Job Image]
  G --> H[Upload Image to WordPress]
  F --> I[Create WordPress Post]
  I --> J[Attach Image to Post]
```

---

## 🧠 AI Agent Role

The LangChain AI Agent is configured to:
- Visit the job posting page,
- Extract structured data (title, description, date, etc.),
- Rewrite the content using a custom prompt,
- Ensure it’s SEO-compliant and Arabic-native,
- Output structured JSON (validated with schema).

---

## 🛠️ Tech Stack

| Service        | Role                            |
|----------------|---------------------------------|
| **n8n**        | Workflow orchestration           |
| **OpenAI**     | LLM for rewriting + extraction   |
| **WordPress**  | Publishing destination           |
| **Regex + JS** | Web scraping & content parsing   |

---

## 🧩 Nodes & Sections

- **HTTP Request** nodes scrape each site.
- **Code** nodes clean & extract job links.
- **AI Agent** sends link + prompt for rewriting.
- **Structured Output Parser** ensures formatted fields.
- **Image download/upload** via HTTP and WordPress media API.
- **Create Post** + **Update Post Image** complete the publishing cycle.

---

## 📂 Custom Prompt Highlights

- Writes posts in Arabic with 0% copied content.
- Uses <h2>, <p>, and <ul> for semantic structure.
- Rewrites titles/meta under 60/160 characters respectively.
- Keeps job facts intact (e.g., salary, dates).
- Ends every post with “طريقة التقديم”.

---

## 📌 Notes

- Make sure API keys (OpenAI, WordPress) are correctly set.
- Validate that the scraped job link contains an actual job posting.
- Prompt is customizable for different job boards or formats.
- Image upload requires base64 or binary format.

---

## ✅ Output Example (Post Fields)

```json
{
  "title": "وظائف شركة XXX 2025 – فرص لحملة البكالوريوس",
  "description": "<h2>أعلنت شركة XXX عن توفر ...</h2> ...",
  "job_link": "https://example.com/job/12345",
  "image": "https://example.com/logo.png",
  "post_date": "2025-08-20",
  "automatic_categorization": {
    "sector": "وظائف حكومية",
    "city": "الرياض",
    "type": "دوام كامل"
  }
}
```

---

## 📅 Update Frequency

- Triggered every 2 minutes (configurable).
- Only latest job post is processed per run.

---

## 📣 Credits

Developed by: [Your Name]  
AI Agent Prompt design & scraping logic by: [Your Brand/Handle]

---
