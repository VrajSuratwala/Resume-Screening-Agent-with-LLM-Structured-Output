# Resume-Screening-Agent-with-LLM-Structured-Output
A Python-based AI pipeline that takes a job description and a resume (PDF or plain text), and outputs a structured evaluation.


# AI Resume Screening Agent

An AI-powered Resume Screening System built using Python, LangChain, Groq LLM, and PDF Resume Parsing.

This project analyzes a candidate's resume against a given Job Description and generates a structured evaluation including:
- Match Score
- Matched Skills
- Missing Skills
- Summary
- Recommendation

The system uses LangChain Prompt Templates, Groq LLM, JSON Output Parsing, and PDF extraction to simulate a lightweight ATS (Applicant Tracking System).

---

# Features

- Upload Resume PDF
- Extract Resume Text Automatically
- Accept Dynamic Job Description Input
- AI-Based Resume Screening
- Structured JSON Output
- Match Score Generation
- Matched Skills Detection
- Missing Skills Detection
- Recommendation System
- Career Suggestions for Low Match Scores
- Error Handling for Invalid PDF/Text
- LangChain Prompt Chaining
- JSON Output Parsing

---

# Technologies Used

| Library / Tool | Purpose |
|---|---|
| Python | Core Programming Language |
| LangChain | Prompt chaining and orchestration |
| Groq API | LLM inference |
| Llama 3.3 70B | Language Model |
| pdfplumber | Extract text from PDF resumes |
| JsonOutputParser | Structured JSON parsing |
| json | JSON formatting |
| Google Colab | Development environment |

---

# Pipeline Architecture

```text
User Enters Job Description
            ↓
User Uploads Resume PDF
            ↓
PDF Resume Text Extraction
            ↓
LangChain Prompt Template
            ↓
Groq LLM (Llama 3.3 70B)
            ↓
JSON Output Parsing
            ↓
Structured Resume Evaluation
            ↓
Final Match Score + Recommendation
