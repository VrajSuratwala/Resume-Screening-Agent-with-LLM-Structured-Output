# Resume-Screening-Agent-with-LLM-Structured-Output

An AI-powered Resume Screening System built using Python, LangChain, Groq LLM, and PDF Resume Parsing.

This project analyzes a candidate’s resume against a given Job Description and generates a structured evaluation similar to a lightweight ATS (Applicant Tracking System).

The system uses:

* LangChain Prompt Templates
* Groq LLM (Llama 3.3 70B)
* Structured JSON Output Parsing
* PDF Resume Extraction
* Pydantic Validation
* Retry Handling with Tenacity

---

# Features

✅ Upload Resume PDF
✅ Extract Resume Text Automatically
✅ Dynamic Job Description Input
✅ AI-Based Resume Screening
✅ Structured JSON Output
✅ Match Score Generation
✅ Matched Skills Detection
✅ Missing Skills Detection
✅ Recommendation System
✅ Career Suggestions for Low Match Scores
✅ Error Handling for Invalid PDFs
✅ Retry Handling for API Failures
✅ Pydantic Output Validation
✅ Token Usage Awareness
✅ Logging Support
✅ LangChain Prompt Chaining
✅ JSON Output Parsing

---

# Technologies Used

| Library / Tool   | Purpose                           |
| ---------------- | --------------------------------- |
| Python           | Core Programming Language         |
| LangChain        | Prompt orchestration and chaining |
| Groq API         | LLM inference                     |
| Llama 3.3 70B    | Language Model                    |
| pdfplumber       | Extract text from PDF resumes     |
| Pydantic         | Structured validation             |
| JsonOutputParser | Structured JSON parsing           |
| Tenacity         | Retry handling for API failures   |
| Logging          | Runtime monitoring and debugging  |
| Google Colab     | Development environment           |

---

# Pipeline Architecture

```text id="’winj0i"
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

# Structured Output Format

The pipeline generates a structured JSON response:

```json id="’winj0j"
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

# How to Run

## 1. Clone Repository

```bash id="’winj0k"
git clone YOUR_GITHUB_REPO_LINK
```

---

## 2. Install Dependencies

```bash id="’winj0l"
pip install -r requirements.txt
```

---

## 3. Configure Groq API Key

In Google Colab:

* Open the left sidebar
* Go to Secrets
* Add:

```text id="’winj0m"
Name: GROQ_API_KEY
Value: YOUR_GROQ_API_KEY
```

Enable Notebook Access.

---

## 4. Run Notebook

Open:

```text id="’winj0n"
Assignment_Final_with_Langchain.ipynb
```

Run all cells sequentially.

---

# Prompt Iterations

## Prompt Version 1

Initial implementation used multiple LLM calls:

```text id="’winj0o"
JD Analysis
→ Resume Analysis
→ Final Evaluation
```

### Problems

* Higher token usage
* Increased latency
* More orchestration complexity
* Intermediate outputs increased failure points

---

## Prompt Version 2 (Final)

Final implementation uses a single structured LangChain pipeline.

### Improvements

* Reduced token cost
* Lower latency
* Simpler architecture
* Easier maintenance
* More reliable structured outputs

---

# Structured Validation

Pydantic models were added for:

* JDAnalysis
* ResumeAnalysis
* FinalEvaluation

The final implementation uses a simplified single-stage LangChain pipeline while preserving structured validation patterns for maintainability and future extensibility.

---

# Retry Handling

Groq API calls are wrapped using Tenacity retries.

This helps recover from:

* temporary API failures
* timeout issues
* rate limits

Retry strategy:

* maximum 3 retries
* exponential backoff delay

---

# Cost Optimization Decisions

Several design choices were intentionally made to reduce LLM token usage and latency:

* Single LLM call instead of multi-stage orchestration
* Recommendation logic computed in Python instead of the LLM
* Resume truncation before inference
* Structured JSON outputs to reduce verbosity
* Retry handling to avoid unnecessary reruns

These optimizations improve:

* response speed
* token efficiency
* API cost control
* reliability

---

# Evaluation Strategy

To test scoring consistency, multiple resumes with different relevance levels were tested against different job descriptions.

The evaluation included:

* strong match resumes
* partial match resumes
* unrelated resumes

Expected score ranges were manually estimated and compared against actual model outputs to evaluate consistency and reliability.

---

# Production Considerations

While this project works well for assignment scope, several production-level concerns were identified:

* OCR fallback would be required for scanned/image-based resumes
* Semantic skill matching should ideally use embeddings instead of prompt rules
* Token and API cost monitoring would be important at scale
* Batch processing would be needed for large recruiter workflows
* Resume PII handling would require compliance review before production deployment

These limitations were intentionally documented to demonstrate production-thinking beyond the demo pipeline.

---

# Test Runs

Evaluation examples are available inside:

```text id="’winj0p"
test_runs/
```

The folder contains:

* strong match cases
* partial match cases
* weak match cases

with:

* expected score ranges
* actual pipeline outputs

---

# Project Structure

```text id="’winj0q"
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
│   ├── ...
│
└── resumes/
```

---

# Future Improvements

Possible future enhancements:

* OCR support for scanned resumes
* Embedding-based semantic skill matching
* Batch resume screening
* FastAPI/Flask deployment
* ATS dashboard UI
* Multi-resume ranking system
* Token cost analytics
* Database integration

---

# Final Outcome

This project demonstrates:

* LLM orchestration
* structured AI pipelines
* prompt engineering
* JSON output parsing
* validation handling
* retry mechanisms
* production-oriented AI system design

using modern AI engineering practices with LangChain and Groq LLM.
