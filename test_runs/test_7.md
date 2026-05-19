
# Test Case 7 — Partial Match

## Job Description

Cloud Engineer with Kubernetes and AWS

## Resume Summary

Candidate has Linux and Docker basics.

## Expected Score Range

50-68

## Why

Some infrastructure overlap.

## Actual Output

```json
{
    "match_score": 40,
    "matched_skills": [
        "Linux",
        "Docker"
    ],
    "missing_skills": [
        "Kubernetes",
        "AWS"
    ],
    "summary": "Weak match, missing key cloud skills",
    "recommendation": "Not a Fit"
}
```

FAIL ❌
Actual Score: 40
Expected Range: 50-68
