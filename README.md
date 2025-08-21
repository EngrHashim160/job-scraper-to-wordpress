# Web Scrap Agent (n8n Workflow)

## 📌 Overview
The **Web Scrap Agent** is an automated workflow built with [n8n](https://n8n.io/) that scrapes job listings from multiple websites, rewrites the content using AI, and publishes the results directly to a WordPress site. It also integrates with Airtable for job post tracking and deduplication.

This agent:
- Fetches the latest job posts from multiple job portals.
- Extracts job title, description, link, image, posting date, and categorization.
- Rewrites the job announcement in **Arabic** with **100% original wording**.
- Uploads job images and creates a WordPress post (with featured image).
- Stores job titles in Airtable to avoid duplicates.
- Runs automatically on a scheduled interval.

---

## ⚙️ Workflow Steps

### 1. **Trigger**
- **Schedule Trigger**: Runs every 2 minutes to check for new jobs.

### 2. **Data Collection**
- **Fetch News List** (`wadhefa.com/news.php`)
- **Get First Web Data** (`ewdifh.com/category/all-jobs`)
- **Get Sec Web Data** (`wdeftksa.com/sa/jobs`)

### 3. **Parsing & Processing**
- **Custom Code Nodes**:
  - Extract job titles, links, dates, and organizations.
  - Normalize and clean URLs and HTML.
  - Select the first/latest job post.
- **Titles Node**: Stores parsed job title and link.

### 4. **Duplicate Check**
- **Airtable (Search Records + Aggregate)**: Fetches the last 9 job titles.
- **AI Model (GPT-4.1-mini)**: Compares new job title with existing titles → returns **Yes** (duplicate) or job title.
- **IF Node**: If duplicate → skip. If unique → proceed.

### 5. **AI Job Rewrite**
- **AI Agent (LangChain + GPT-4.1-mini)**:
  - Extracts full job details from the job link.
  - Rewrites the announcement in **Arabic** (400–550 words).
  - Ensures SEO-friendly titles and meta descriptions.
  - Generates structured job posting (title, description, conditions, benefits, etc.).

### 6. **Media Handling**
- **Download Image** from job post.
- **Upload Image** to WordPress (via REST API).
- **Update Post Featured Image**.

### 7. **Publishing**
- **Create WordPress Post** (draft or published).
- Post includes rewritten description, structured sections, and featured image.

### 8. **Storage**
- **Airtable (Create Record)**: Saves the job title for future duplicate checks.

---

## 📂 Integrations
- **Websites**: wadhefa.com, ewdifh.com, wdeftksa.com  
- **AI**: OpenAI GPT-4.1-mini (via LangChain in n8n)  
- **Database**: Airtable (for job title history)  
- **CMS**: WordPress (REST API for posts & media)  

---

## 🔧 Requirements
- [n8n](https://n8n.io/) running instance
- **Credentials**:
  - OpenAI API Key
  - Airtable Personal Access Token
  - WordPress REST API credentials (username + application password or token)

---

## 🚀 Setup Instructions
1. Import the JSON workflow into your n8n instance.
2. Configure credentials for:
   - **OpenAI**
   - **Airtable**
   - **WordPress**
3. Adjust the **schedule trigger interval** if needed.
4. Modify **WordPress endpoint URL** and **authorization token**.
5. Enable the workflow.

---

## 📊 Workflow Visualization
- **Step 1**: Trigger  
- **Step 2**: Fetch job sources  
- **Step 3**: Extract first/latest job post  
- **Step 4**: Check duplicates in Airtable  
- **Step 5**: Rewrite job post in Arabic with AI  
- **Step 6**: Upload image + create WordPress draft  
- **Step 7**: Update post with featured image  
- **Step 8**: Save job title to Airtable  

---

## 🛠️ Customization
- Add/remove job sources by updating HTTP Request nodes.  
- Adjust AI rewriting rules (system prompt in `AI Agent`).  
- Change publishing behavior (draft vs. auto-publish).  
- Modify schedule interval (default: every 2 minutes).  

---

## ✅ Benefits
- Fully automated scraping → rewriting → publishing pipeline.  
- SEO-friendly Arabic job announcements.  
- Eliminates duplicate posts using Airtable + AI.  
- Saves time and ensures professional formatting.  
