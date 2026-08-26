# AI Resume Job Matcher (n8n Workflow)

An n8n automation that scores how well a resume matches a job description using AI, and returns a clean, structured match report — matching skills, missing skills, strengths, and improvement suggestions.

## 🖼️ Demo

![AI Resume Job Matcher screenshot](screenshots/workflow-overview.png)

> Add your screenshot(s) to a `screenshots/` folder in the repo root, then reference them here (see [Adding Screenshots](#-adding-screenshots) below).

## ✨ Features

- **Simple web form intake** — users upload a resume (PDF) and provide a job description either as pasted text or a PDF upload.
- **Automatic PDF text extraction** for both resume and job description files.
- **AI-powered analysis** using GPT-5-mini via a structured JSON schema, returning:
  - `match_score` (0–100)
  - `matching_skills` — skills demonstrated in the resume that align with the job
  - `missing_skills` — important job skills not evidenced in the resume
  - `strengths` — candidate's strongest relevant areas
  - `suggestions` — specific resume improvement tips for that job
- **Grounded output** — the prompt explicitly instructs the model not to invent skills, experience, or metrics; only resume-evidenced items count as matches.
- **Conditional branching** — routes results into a "Strong Match" (score ≥ 80) or "Needs Improvement" path, each formatted differently (the Strong Match view omits missing skills).
- **Clean final report** rendered back to the user in the n8n form as a readable, formatted summary.

## 🧩 How It Works

1. **On form submission** — user fills out the form (Resume PDF required; Job Description as text and/or PDF).
2. **extract Resume** / **extract JD** — extracts raw text from the uploaded PDF(s).
3. **Prepare Resume** / **Prepare JD** — normalizes extracted text into consistent fields.
4. **Merge1** — combines resume and job description data into a single item.
5. **Edit Fields** — sets `resume_text` and `job_description` (falls back to the JD PDF text if no text was pasted).
6. **Message a model** — sends both to GPT-5-mini with a structured JSON schema prompt for scoring and analysis.
7. **flatten AI** — parses the model's JSON response.
8. **If** — checks whether `match_score >= 80`.
9. **Strong Match** / **Needs Improvement** — formats the result for each branch.
10. **Merge** — recombines both branches into one path.
11. **Create clean report** — builds a readable plain-text report (score, matching/missing skills, strengths, recommendations).
12. **Form (completion)** — displays the final report to the user.

## 📋 Requirements

- [n8n](https://n8n.io/) instance (self-hosted or cloud)
- An OpenAI API credential configured in n8n with access to `gpt-5-mini` (or update the model in the **Message a model** node to one you have access to)
- No external database required — this is a stateless, form-in / form-out workflow

## 🚀 Setup

1. Clone this repository:
   ```bash
   git clone https://github.com/<your-username>/<your-repo>.git
   ```
2. Open your n8n instance and go to **Workflows → Import from File**.
3. Select `ai-resume-job-matcher-github.json` from this repo.
4. Add/select your OpenAI credentials in the **Message a model** node.
5. Activate the workflow.
6. Open the generated form URL (from the **On form submission** trigger) to test it.

## 📁 Repository Structure

```
.
├── ai-resume-job-matcher-github.json   # n8n workflow export
├── screenshots/                        # workflow & UI screenshots
│   └── workflow-overview.png
└── README.md
```

## 🖼️ Adding Screenshots

To include screenshots in your GitHub repo:

1. Create a `screenshots/` folder at the root of your repository.
2. Add your image file(s) there, e.g. `screenshots/workflow-overview.png`, `screenshots/form-ui.png`, `screenshots/sample-report.png`.
3. Reference them in this README using standard Markdown image syntax:
   ```markdown
   ![Workflow overview](screenshots/workflow-overview.png)
   ```
4. Commit and push both the image and the updated README — GitHub will render the images automatically on the repo page.

You can include multiple screenshots (e.g., the n8n canvas, the intake form, and a sample output report) as separate images or side by side using an HTML `<table>` if you want a grid layout.

## ⚠️ Notes

- The AI is explicitly instructed not to fabricate skills or experience — matches are only counted when evidenced in the resume text.
- The workflow currently accepts only `.pdf` resumes; job descriptions can be pasted as text or uploaded as `.pdf`.

## 📄 License

Add your preferred license here (e.g., MIT).
