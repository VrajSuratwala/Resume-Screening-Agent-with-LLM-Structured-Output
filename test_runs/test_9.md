
# Test Case 9 — Weak Match

## Job Description

Cybersecurity Analyst

## Resume Summary

Candidate is Android developer.

## Expected Score Range

20-35

## Why

Minimal security overlap.

## Actual Output

```json
{
    "match_score": 20,
    "matched_skills": [],
    "missing_skills": [
        "Cybersecurity",
        "Security Analysis",
        "Threat Detection",
        "SQL",
        "Flask",
        "Networking"
    ],
    "summary": "Weak match due to lack of relevant cybersecurity experience",
    "recommendation": "Not a Fit"
}
```

PASS ✅
Actual Score: 20
Expected Range: 20-35
