# Automated Job Search & Cover Letter Generator with Match Scoring (n8n + Google Gemini)

This project is a fully automated workflow that simplifies the job application process using **n8n** and **Google Gemini**.  
It fetches job postings from LinkedIn, evaluates how well they match your resume and skills, and then generates a tailored cover letter.  
All results are stored in Google Sheets for tracking, and if the matching score meets your criteria, the workflow will even prepare and send an application email directly to your inbox.  

---

## 📌 Project Objectives

- Automate repetitive tasks in the job search process.  
- Identify jobs that best fit your skills and experience.  
- Generate AI-powered, personalized cover letters for each application.  
- Store all job details and results in Google Sheets for easy tracking.  
- Save time and improve efficiency in job hunting.  

---

## 🚀 Workflow Overview

Below is the step-by-step flow of the workflow:

### 1. **Scheduler + RSS Read**
- The workflow begins with a scheduler trigger that runs at defined intervals (for example, daily).  
- It uses the **RSS Read** node to fetch the latest LinkedIn job postings.  
- The feed is limited to **1 job at a time** to avoid overwhelming the system.

### 2. **HTTP Request**
- Takes the job link from the RSS feed and performs an HTTP request.  
- Extracts the raw job description and related information.

### 3. **Google Gemini (Company Data Extraction)**
- Uses Gemini to analyze the job description.  
- Separates important details such as:  
  - Company name  
  - Job title  
  - Role requirements  
  - Skills needed  

### 4. **Code (Structured Output)**
- Since Gemini’s response is in JSON but may not always be properly structured, this step uses code to clean and format the output.  
- Converts the response into a **consistent JSON structure** that can be used later in the pipeline.

### 5. **Google Gemini (Match Scoring)**
- Compares the **job requirements** with your **resume and skills**.  
- Generates a **matching score** to indicate how well you fit the role.

### 6. **Google Gemini (Cover Letter Generator)**
- If the job is relevant, Gemini creates a **personalized cover letter**.  
- The letter highlights your skills and aligns them with the job requirements.

### 7. **Google Sheets (Store Data)**
- All extracted and generated data is stored in Google Sheets, including:  
  - Job title and company  
  - Requirements and extracted details  
  - Matching score  
  - Generated cover letter  

### 8. **Get Rows (Google Sheets)**
- Retrieves saved data for further processing or review.  
- Ensures that the latest job entry is accessible.

### 9. **IF Node (Score Check)**
- Compares the **matching score** with a pre-defined threshold (e.g., 70%).  
- If the score is **greater than the threshold**, the workflow continues.  
- Otherwise, the job is ignored.

### 10. **Code (Mail Body)**
- A code node generates a well-structured **email body**.  
- Combines the cover letter, job details, and formatted message into a ready-to-send application.

### 11. **Email Node**
- The final step sends the email to your **personal inbox**.  
- This allows you to review or directly forward it as your job application.

---

## 🛠️ Tech Stack

- **n8n** – Workflow automation  
- **Google Gemini** – AI-powered NLP for job description analysis, scoring, and cover letter generation  
- **Google Sheets** – Storage and retrieval of job data  
- **RSS Feed (LinkedIn)** – Job fetching  
- **Email (SMTP)** – Sending structured application mails  

---

## ✨ Key Features

- Automated fetching of LinkedIn job postings via RSS feed.  
- One-job-at-a-time processing to ensure accuracy.  
- Match scoring between job requirements and your resume/skills.  
- Tailored cover letter generation using Google Gemini.  
- Centralized tracking of job applications in Google Sheets.  
- Conditional application (only apply if matching score passes threshold).  
- Auto-generated email body for job applications.  

---

## ⚡ How to Use

1. Clone this repository.  
2. Import the workflow into **n8n**.  
3. Set up credentials for:  
   - LinkedIn RSS feed  
   - Google Gemini API  
   - Google Sheets API  
   - Email SMTP (for sending mails)  
4. Configure your **resume and skill data** inside the workflow.  
5. Adjust the **matching score threshold** in the IF node according to your preference.  
6. Start the scheduler and let the workflow automate your job search process.  

---

## 📊 Workflow Visualization

```mermaid
flowchart TD
    A[Scheduler] --> B[RSS Read LinkedIn Jobs]
    B --> C[HTTP Request: Fetch Job Details]
    C --> D[Google Gemini: Extract Company & Requirements]
    D --> E[Code: Structure JSON Output]
    E --> F[Google Gemini: Match Scoring with Resume]
    F --> G[Google Gemini: Generate Cover Letter]
    G --> H[Google Sheets: Store Data]
    H --> I[Google Sheets: Get Rows]
    I --> J{Match Score > Threshold?}
    J -- Yes --> K[Code: Generate Email Body]
    K --> L[Email Node: Send Application Mail]
    J -- No --> M[End Workflow]
