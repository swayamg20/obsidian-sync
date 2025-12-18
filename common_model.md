The Mental Model
```
Human Language
   ↓
Decision Intelligence Core  ← %% reusable asset %%
   ↓
Domain Execution
```

The Decision Intelligence Core must be:
- cheap
- fast
- stable
- domain-agnostic
- explainable

### Common Model
The common model is an **SLM (3B–7B)** that is **fine-tuned to do ONE thing extremely well**:
> ***Convert messy human language into structured decision state.***

 **Inputs:**
- Free-form text
- Hesitation
- Partial info
- Emotional cues

**Outputs (structured, always):**

```
{
    "goal_clarity": 0.62,
    "risk_tolerance": "low",
    "budget_anxiety": "high",
    "time_flexibility": "medium",
    "energy_preference": "low",
    "confidence_score": 0.41,
    "dominant_tradeoff": "comfort_vs_cost"
}
```

| Task                  | Model          |
| --------------------- | -------------- |
| Intent extraction     | Fine-tuned SLM |
| Emotion detection     | Fine-tuned SLM |
| Confidence scoring    | Fine-tuned SLM |
| Preference memory     | SLM + symbolic |
| Trade-off explanation | LLM            |
| Planning              | LLM + tools    |
| Re-assurance          | LLM            |
| Edge / offline use    | SLM            |
keep:
- **same SLM**
- same decision schema
- same confidence model
swap:
- domain knowledge
- tools
- constraints
