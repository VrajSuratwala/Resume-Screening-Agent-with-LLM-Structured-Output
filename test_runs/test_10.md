
# Test Case 10 — Weak Match

## Job Description

Backend Python Developer

## Resume Summary

Candidate is graphic designer.

## Expected Score Range

10-30

## Why

Very weak backend overlap.

## Actual Output

```json
{
    "match_score": 0,
    "matched_skills": [],
    "missing_skills": [
        "Python",
        "Backend development"
    ],
    "summary": "No relevant experience",
    "recommendation": "Not a Fit"
}
```

FAIL ❌
Actual Score: 0
Expected Range: 10-30
