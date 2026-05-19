# Resume Screening Agent with LLM + Structured Output

An AI-powered Resume Screening System built using Python, LangChain, Groq LLM, and PDF Resume Parsing.

This project analyzes a candidate's resume against a given Job Description and generates a structured evaluation similar to a lightweight ATS (Applicant Tracking System).

---

## Features

- Upload Resume PDF
- Extract Resume Text Automatically
- Dynamic Job Description Input
- AI-Based Resume Screening
- Structured JSON Output
- Match Score Generation (0–100)
- Matched Skills Detection
- Missing Skills Detection
- Recommendation System (Strong Fit / Potential Fit / Not a Fit)
- Career Suggestions for Low Match Scores
- Error Handling for Invalid PDFs
- Retry Handling for API Failures
- Pydantic Output Validation
- Token Usage Awareness
- Logging Support

---

## Technologies Used

| Library / Tool   | Purpose                           |
|------------------|-----------------------------------|
| Python           | Core Programming Language         |
| LangChain        | Prompt orchestration and chaining |
| Groq API         | LLM inference                     |
| Llama 3.3 70B    | Language Model                    |
| pdfplumber       | Extract text from PDF resumes     |
| Pydantic         | Structured output validation      |
| JsonOutputParser | Structured JSON parsing           |
| Tenacity         | Retry handling for API failures   |
| Logging          | Runtime monitoring and debugging  |
| Google Colab     | Development environment           |

---

## Pipeline Architecture

```text
User Enters Job Description
            ↓
User Uploads Resume PDF
            ↓
PDF Resume Text Extraction
            ↓
Resume Text Validation
            ↓
LangChain Prompt Template
            ↓
Groq LLM (Llama 3.3 70B)
            ↓
Structured JSON Parsing
            ↓
Pydantic Validation
            ↓
Final Resume Evaluation
            ↓
Match Score + Recommendation
```

---

## Structured Output Format

```json
{
    "match_score": 88,
    "matched_skills": [
        "Python",
        "Flask",
        "SQL"
    ],
    "missing_skills": [
        "Docker"
    ],
    "summary": "Candidate has strong backend development experience with relevant API and database skills.",
    "recommendation": "Strong Fit"
}
```

---

## How to Run

### 1. Clone Repository

```bash
git clone https://github.com/VrajSuratwala/Resume-Screening-Agent-with-LLM-Structured-Output
```

### 2. Install Dependencies

```bash
pip install -r requirements.txt
```

### 3. Configure Groq API Key

In Google Colab:
- Open the left sidebar
- Go to **Secrets**
- Add: 
Name: GROQ_API_KEY
Value: YOUR_GROQ_API_KEY

Enable **Notebook Access**.

### 4. Run Notebook

Open `Assignment_Final_with_Langchain.ipynb` and run all cells sequentially.

---

## Prompt Iterations

### Version 1 — First Attempt

```text
You are a resume screener.
Given a job description and resume, evaluate the candidate.
Return a JSON with match_score, matched_skills, missing_skills, summary.

Job Description: {job_description}
Resume: {resume_text}
```

**Problems noticed:**
- LLM sometimes returned plain text instead of JSON
- No scoring guidance — same resume scored 71 one run, 84 the next
- "Evaluate the candidate" was too vague — model invented its own criteria
- `matched_skills` sometimes returned as a string instead of a list

---

### Version 2 — Final (With Scoring Rules + Format Enforcement)

```text
You are an advanced AI Resume Screening Assistant.

Compare the candidate resume with the job description carefully.

IMPORTANT:
- Return ONLY valid JSON
- Do NOT return markdown
- Do NOT return explanation outside JSON

SCORING RULES:
- 80-100 = strong match
- 60-79 = moderate match
- below 60 = weak match

SEMANTIC MATCHING:
- SQLite counts as partial SQL experience
- FastAPI counts partially similar to Flask

OUTPUT FORMAT:
{
    "match_score": 0,
    "matched_skills": [],
    "missing_skills": [],
    "summary": ""
}

JOB DESCRIPTION:
{job_description}

RESUME:
{resume_text}
```

**What improved:**
- Explicit score bands made output consistent across runs
- `Return ONLY valid JSON` reduced format errors significantly
- Semantic matching rules helped partial skill overlaps score fairly
- Output format example anchors the model to the correct structure

**Key lesson:** Vague instructions give vague outputs. Every constraint added
to the prompt is one less thing the model guesses wrong.

---

## Known Limitations

- **Scanned PDFs will fail** — pdfplumber cannot extract text from image-based
  resumes. Pipeline raises a ValueError with a clear message. OCR fallback
  (e.g. pytesseract) would be needed in production.
- **Vague job descriptions produce inconsistent scores** — if the JD has no
  specific skills listed, the model has little to compare against.
- **Semantic matching is hardcoded in the prompt** — rules like
  "SQLite = partial SQL" don't scale. Embedding-based similarity would be the
  production fix.
- **`JDAnalysis` and `ResumeAnalysis` Pydantic models are defined but not
  wired into the current single-call pipeline** — they exist for future
  extensibility if a multi-stage pipeline is needed.
- **No batching** — currently processes one resume per run. Real recruiter
  workflows need batch + parallel processing.

---

## Evaluation Results

10 test cases were run covering strong, partial, and weak match scenarios.
Sample results:

**Test 1 — Strong Match**
- JD: Python Backend Developer with Flask, SQL, REST APIs
- Resume: Python developer with Flask, SQL, REST APIs
- Expected: 85–95 | Actual: 90 | ✅ PASS

**Test 5 — Partial Match**
- JD: Frontend React Developer
- Resume: Frontend developer with HTML, CSS, JavaScript
- Expected: 60–72 | Actual: 60 | ✅ PASS

**Test 10 — Weak Match**
- JD: Backend Python Developer
- Resume: Graphic designer with Canva and UI portfolio
- Expected: 10–30 | Actual: 0 |  FAIL

Full results with JSON outputs are available in `test_runs/`.

---

## Project Structure

```text
Resume-Screening-Agent/
│
├── Assignment_Final_with_Langchain.ipynb
├── README.md
├── requirements.txt
│
├── outputs/
│   └── sample_output.json
│
├── test_runs/
│   ├── test_1.md
│   ├── test_2.md
│   ├── test_3.md
│   └── ...
│
└── resumes/
```

---

## Retry Handling

Groq API calls are wrapped using Tenacity with:
- Maximum 3 retries
- Exponential backoff (1s → 10s)

Handles temporary API failures, timeouts, and rate limits.

---

## Cost Optimization Decisions

- Single LLM call instead of multi-stage orchestration
- Recommendation logic computed in Python, not the LLM
- Resume truncated to 12,000 characters before inference
- Structured JSON output reduces response verbosity

---

## Production Considerations

| Concern | Current State | Production Fix |
|---|---|---|
| Scanned PDFs | Raises ValueError | Add OCR fallback |
| Skill matching | Hardcoded prompt rules | Embeddings + cosine similarity |
| Batch processing | Single resume per run | Queue + parallel workers |
| PII handling | Raw resume sent to Groq | Redaction before API call |
| Cost monitoring | Token estimate logged | Full cost tracking per run |

---

## Future Improvements

- OCR support for scanned resumes
- Embedding-based semantic skill matching
- Batch resume screening
- FastAPI or Flask deployment
- ATS dashboard UI
- Multi-resume ranking system
- Database integration for results storage
