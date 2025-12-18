"Trip planning is not an information problem.  
It is a _confidence formation_ problem."

So the AI is not answering questions.  
It is **helping the user converge mentally**
# *convergence*
**Approach**
User → expresses confusion → AI **shrinks possibility space**
#### 1. Progressive Constraint Induction

Instead of asking explicit filters (“budget?”, “dates?”), the AI:
- Listens for _implicit constraints_
    
- Converts language → latent variables
User: “I just want a break, nothing hectic”
Analysis:
```
{
  "energy_tolerance": "low",
  "novelty_preference": "medium",
  "pace": "slow",
  "crowd_aversion": "high"
}
```
- Fine-tune an **SLM (3–7B)** on:
    - Travel intent utterances
        
    - Emotional travel language
- Output a ***latent intent graph***, not text

LLM = reasoning  
SLM = intent extractor (cheap, fast, on-device later)
### 2. Example of Agents

- Decision Fatigue Agent
- Budget Anxiety Agent
- Energy & Pace Agent    
- Risk & Uncertainty Agent
- Social Friction Agent (group trips)

Each agent should do these:
1. Scores the plan
2. Raises objections
3. Suggests adjustments

### 3. LLMs for **Reasoned Trade-off Explanations**

People don’t need “best plan”.  
They need **why this plan is best for _them_**.

**Use LLMs to generate:**
- Counterfactual explanations:
    - “If you chose Goa instead, you’d save ₹8k but lose quiet mornings.”        
- Regret-minimizing narratives:
    - “Given your low tolerance for crowds, this choice is safer.”
This is _decision psychology_, not search.

### 4. Memory as a First-Class System

Types of memory:
1. Preference memory (stable)
2. Trip-specific memory (ephemeral)
3. Stress triggers (very powerful)

```
Example:
> User previously said: “I hate rushing mornings”
Future trips auto-adjust:
- No early departures 
- AI explains: “I avoided early starts because you’ve disliked them before.”
```

- Vector DB + symbolic memory
- Periodic LLM summarization → structured preference schema

## High Level Design

```
User
 ↓
Conversational Layer (LLM)
 ↓
Intent Extractor (SLM fine-tuned)
 ↓
Psychological Agent Ensemble
 ↓
Constraint Solver + Knowledge Graph
 ↓
Explanation Generator (LLM)
 ↓
Memory Update
```
