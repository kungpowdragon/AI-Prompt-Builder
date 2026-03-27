# A Practitioner's Reference — From First Principles to Power Moves

> **The core idea:** Prompt engineering is structural communication and constraint management. You are not casting spells — you are writing a spec for a probabilistic engine. The clearer the spec, the better the output. Every technique in this document is a lever for reducing ambiguity.

---

## 0. The Right Mindset

Think like a **Product Manager writing a spec**, not a magician hoping for magic words.

Before writing any prompt, answer four questions:

- Who is speaking? _(Role)_
- Who is the audience? _(User / Tone)_
- What exactly should the output look like? _(Format)_
- What counts as success? _(Constraints)_

**Treat the AI like a brilliant intern:** vast knowledge, zero context about your situation, and prone to confident mistakes. Give it the background, the format, and a way to say "I don't know" — and it performs well.

---

## 1. Core Architecture — System vs. User Prompts

Most modern LLM APIs separate prompt types. Respect this separation.

|Component|Purpose|What Belongs Here|
|:--|:--|:--|
|**System Prompt**|Defines the AI's identity, rules, and global behavior|Persona, output format, "never-do" rules, tone|
|**User Prompt**|Provides the specific task and data for this interaction|Objective, input data, specific instructions|

> **Mental Model:** System Prompt = the job description. User Prompt = today's task.

**System prompt example:**

```
SYSTEM: You are a concise technical interviewer. Ask one follow-up question 
at a time. Never give the full answer; guide the user toward it.
```

**User prompt example:**

```
USER: Help me learn. Explain how HTTPS works.
```


Use system prompts to set role and tone, specify policies (what not to do), and define default output formats (JSON, Markdown, etc.).

---

## 2. The Universal Prompt Template _(Copy-Paste Ready)_

Fill in the square brackets. Skip sections that don't apply, but never skip **OBJECTIVE** or **OUTPUT FORMAT**.

```
ROLE: You are an experienced [ROLE, e.g., senior backend engineer].

CONTEXT:
- [Background: project, constraints, environment]
- Audience: [who will read or use this output]

OBJECTIVE:
Your task is to [specific goal: design an API, write a test plan, debug a function, etc.].

CONSTRAINTS:
- Use the following only: [technologies, sources, scope]
- Do not [X, Y, Z]
- Follow this format exactly:
  1. ...
  2. ...

STYLE:
- Tone: [formal / casual / technical / dry]
- Length: [max N words / max 1 page / approximately 200 words]
- Format: [Markdown / JSON / plain text / bullet points]

EXAMPLES (optional but high-value):
[1–3 short input → output examples]

INPUT:
[Paste your actual input: code, requirements, error message, data, etc.]

OUTPUT:
Produce the result following the constraints and format above.
```

---

## 3. Prompting Frameworks

Use these as mental checklists to ensure you haven't left out critical information. **Pick one per task, you don't need all of them.**

|Framework|Components|Best For|
|:--|:--|:--|
|**CO-STAR**|Context, Objective, Style, Tone, Audience, Response|Comprehensive tasks: blogs, reports, analyses|
|**AUTOMAT**|Act as, User persona, Targeted action, Output definition, Mode/tone, Atypical cases, Topic whitelisting|Complex API prompts, system design|
|**CRAFT**|Context, Role, Action, Format, Tone|General-purpose, most everyday tasks|
|**RACE**|Role, Action, Context, Expectation|Technical audits, security reviews, evaluations|
|**RTF**|Role, Task, Format|Quick, structured outputs: code, lists, tables|
|**ROSES**|Role, Objective, Scenario, Expected Solution, Steps|Complex workflows, project planning|
|**CARE**|Context, Action, Result, Example|Business communication, case studies, emails|
|**CRISPE**|Context, Role, Instruction, Steps, Personality, Example|Data analysis, detailed reports|
|**APE**|Action, Purpose, Expectation|Simple, focused tasks like emails or summaries|

---

### CO-STAR in Practice

_Best for: comprehensive content — reports, essays, long-form writing._

|Element|Filled In|
|:--|:--|
|**Context**|I'm writing for a non-technical executive audience at a mid-size SaaS company.|
|**Objective**|Explain why we should invest in a data warehouse this quarter.|
|**Style**|Executive briefing — clear, structured, no fluff.|
|**Tone**|Confident and direct, not alarmist.|
|**Audience**|C-suite; assume no technical background but high business literacy.|
|**Response**|400 words max. Use headers: Problem, Solution, ROI. End with a single recommended action.|

**Resulting prompt:**

```
Context: I'm writing an internal briefing for C-suite executives at a mid-size SaaS company.
Objective: Make the case for investing in a data warehouse this quarter.
Style: Executive briefing — structured, no jargon, no filler.
Tone: Confident and direct.
Audience: Non-technical leaders with strong business literacy.
Response: 400 words max. Use three headers — Problem, Solution, ROI — and end with 
one recommended action.
```

---

### AUTOMAT in Practice

_Best for: complex system prompts, API configurations, multi-constraint tasks._

|Letter|Element|Filled In|
|:--|:--|:--|
|**A**|Act as a…|Senior software architect|
|**U**|User persona & audience|Junior developer who knows Python but not distributed systems|
|**T**|Targeted action|Design a high-level architecture for a URL-shortening service|
|**O**|Output definition|1-paragraph overview + Mermaid diagram + tech stack with pros/cons|
|**M**|Mode / tone / style|Concise and professional, bullet points for lists|
|**A**|Atypical cases|If requirements are ambiguous, list 3 clarifying questions before proceeding|
|**T**|Topic whitelisting|Backend only — do not address frontend frameworks|

**Resulting prompt:**

```
You are a senior software architect explaining to a junior Python developer 
who has no distributed systems experience.

Design a high-level architecture for a URL-shortening service.

Return:
1. A 1-paragraph plain-English overview
2. A diagram in Mermaid syntax
3. A table of technologies with pros and cons

Use a concise, professional tone. Bullet points for lists.
If anything about the requirements is unclear, list 3 questions 
you'd ask before designing. Backend only — ignore frontend concerns.
```

---

### CRAFT in Practice

_Best for: everyday tasks where you need a clean result without over-engineering the prompt._

| Element     | Filled In                                                                     |
| :---------- | :---------------------------------------------------------------------------- |
| **Context** | I'm a non-technical founder trying to hire my first engineer.                 |
| **Role**    | Act as a technical recruiter with experience hiring for early-stage startups. |
| **Action**  | Explain what a "full-stack React/Node developer" actually does day-to-day.    |
| **Format**  | Bullet points, using plain analogies, no acronyms without explanation.        |
| **Tone**    | Friendly, encouraging, no condescension.                                      |

**Resulting prompt:**

```
Context: I'm a non-technical founder hiring my first engineer.
Role: Act as a technical recruiter experienced with early-stage startups.
Action: Explain what a full-stack React/Node developer actually does day-to-day.
Format: Bullet points. Use plain analogies. Spell out any acronyms.
Tone: Friendly and encouraging.
```

---

### RACE in Practice

_Best for: audits, reviews, risk assessments — tasks with a defined success criterion._

|Element|Filled In|
|:--|:--|
|**Role**|Senior security auditor|
|**Action**|Review this Python API endpoint for security vulnerabilities|
|**Context**|Production environment, public-facing, handles user PII|
|**Expectation**|List the top 3 risks with severity scores (CVSS), and one mitigation per risk|

**Resulting prompt:**

```
Role: You are a senior security auditor.
Action: Review the following Python API endpoint for security vulnerabilities.
Context: This is a public-facing production endpoint that handles user PII. 
High-traffic, no WAF currently in place.
Expectation: Return the top 3 risks. For each: name, CVSS severity score (1–10), 
and one specific mitigation step.

[Paste code here]
```

---

### RTF in Practice

_Best for: quick, structured outputs where you just need a clean result fast._

|Element|Filled In|
|:--|:--|
|**Role**|Python expert|
|**Task**|Write a function to deduplicate a list of dictionaries by a given key|
|**Format**|Single code block, no explanation, include a docstring|

**Resulting prompt:**

```
Role: Python expert.
Task: Write a function that deduplicates a list of dictionaries by a specified key.
Format: Single code block only. Include a docstring. No explanation outside the block.
```

RTF is deliberately minimal. If you find yourself wanting to add more — switch to CRAFT or AUTOMAT.

---

### ROSES in Practice

_Best for: multi-step workflows, project planning, tasks with a defined process._

|Element|Filled In|
|:--|:--|
|**Role**|Product manager at a B2B SaaS company|
|**Objective**|Launch a new onboarding flow for enterprise clients|
|**Scenario**|Current onboarding takes 3 weeks; churn in the first 30 days is at 18%|
|**Expected Solution**|A revised onboarding plan that reduces time-to-value to under 7 days|
|**Steps**|Break the plan into: Audit, Redesign, Pilot, Measure|

**Resulting prompt:**

```
Role: You are a senior product manager at a B2B SaaS company.
Objective: Design a new enterprise client onboarding flow.
Scenario: Current onboarding takes 3 weeks. First-30-day churn is 18%, 
largely attributed to slow time-to-value.
Expected Solution: A revised plan that reduces time-to-first-value to under 7 days.
Steps: Structure your response in four phases — Audit (what's broken), 
Redesign (proposed changes), Pilot (how to test it), Measure (what success looks like).
```

---

### CARE in Practice

_Best for: business communication, persuasive writing, case studies, client-facing content._

|Element|Filled In|
|:--|:--|
|**Context**|A client is hesitant to renew after a rough Q3 with delayed deliverables|
|**Action**|Write a renewal email that acknowledges the issues and makes a compelling case to stay|
|**Result**|The client agrees to a 12-month renewal|
|**Example**|Tone should mirror a trusted advisor, not a salesperson — here's a sample opener I like: "I want to be direct with you about what happened and what we've changed."|

**Resulting prompt:**

```
Context: A long-standing client is hesitant to renew after delays hurt their Q3 results. 
They've expressed frustration but haven't churned yet.
Action: Write a renewal email from their account director.
Result: The goal is a signed 12-month renewal.
Example: Tone = trusted advisor, not salesperson. Direct acknowledgment 
of the problem, then pivot to what changed. Opener style: 
"I want to be direct with you about what happened and what we've changed."
```

---

### CRISPE in Practice

_Best for: analytical tasks that require a clear thinking process — data reports, research synthesis, structured assessments._

|Element|Filled In|
|:--|:--|
|**Context**|Q3 sales data across 4 regions, provided as a CSV|
|**Role**|Senior data analyst|
|**Instruction**|Identify underperforming regions and the likely contributing factors|
|**Steps**|1. Summarize the data. 2. Identify outliers. 3. Hypothesize causes. 4. Recommend actions.|
|**Personality**|Professional and precise — no hedging, no filler|
|**Example**|Output should look like a consulting slide deck: short headline finding, supporting evidence, recommendation|

**Resulting prompt:**

```
Context: I'm providing Q3 sales data across 4 regions as a CSV.
Role: You are a senior data analyst.
Instruction: Identify which regions are underperforming and why.
Steps: Follow this structure — 1) Summarize the data, 2) Identify outliers, 
3) Hypothesize contributing factors, 4) Recommend actions.
Personality: Professional and precise. No hedging. No filler phrases.
Example: Format each finding like a consulting slide — 
bold headline, 2–3 supporting data points, one clear recommendation.
```

---

### APE in Practice

_Best for: simple, single-objective tasks — especially when you're in a hurry._

|Element|Filled In|
|:--|:--|
|**Action**|Draft an invitation email|
|**Purpose**|Invite a prospective client to a product demo next week|
|**Expectation**|Formal tone, under 150 words, end with a clear CTA and a scheduling link placeholder|

**Resulting prompt:**

```
Action: Draft an invitation email.
Purpose: Invite a prospective enterprise client to a product demo next week.
Expectation: Formal tone. Under 150 words. End with a clear call-to-action 
and a placeholder for a scheduling link: [SCHEDULING LINK].
```

APE is the "if in doubt, just start here" framework. It forces you to articulate the minimum viable spec: what, why, and what good looks like.

---

## 4. Structural Delimiters — XML vs. Markdown

Delimiters prevent **prompt injection** and help the model distinguish between instructions and data. They are most important in long, multi-part prompts.

### XML Tags _(Recommended for Claude & complex tasks)_

XML handles nested structures cleanly and is rarely confused with content.

```xml
<instructions>
  Summarize the following document.
</instructions>
<constraints>
  Under 100 words. No jargon.
</constraints>
<context>
  [Paste document here]
</context>
```

### Markdown & Triple Quotes _(Recommended for GPT-4+ & simple tasks)_

```markdown
### Instructions
Summarize the text below.

### Text
"""
[Insert text here]
"""
```

### When to use which

- **Long prompts with many parts** → XML always wins for clarity.
- **Short, conversational prompts** → Markdown is sufficient.
- **Pipelines and automation** → XML; it's machine-parseable.

---

## 5. Prompting Techniques

### 5.1 Zero-Shot vs. Few-Shot

|Technique|What It Is|When to Use|
|:--|:--|:--|
|**Zero-Shot**|Direct instruction, no examples|Simple tasks: classify, translate, basic Q&A|
|**Few-Shot**|2–5 input → output examples before the task|Non-trivial tasks; when format or style must be exact|

> **Rule of thumb:** If a human would need an example to do this task correctly, the model definitely does. Use Few-Shot.

**Few-shot template:**

```
You are an expert [ROLE].

Examples:
INPUT: [example 1 input]
OUTPUT: [example 1 output]

INPUT: [example 2 input]
OUTPUT: [example 2 output]

Now do the same for:
INPUT: [your actual input]
OUTPUT:
```

**Worked few-shot example (JSON parsing):**

```
Input: "I want a small pizza with cheese and tomato sauce."
Output: {"size": "small", "ingredients": ["cheese", "tomato sauce"]}

New task: Parse "I want a large pizza with pepperoni."
```

---

### 5.2 Chain-of-Thought (CoT)

Force the model to reason before answering. Dramatically reduces errors in math, logic, and multi-step reasoning.

**Trigger phrases:**

- _"Think step-by-step before giving the final answer."_
- _"Break this down logically, then provide the answer."_
- _"Work through the problem step-by-step, but only return the final answer."_ _(hidden reasoning)_

**CoT example:**

```
Problem: When I was 3, my partner was 3× my age. I'm 20 now. How old is my partner?

Steps:
1. Partner's age when I was 3: 3 × 3 = 9
2. Age gap: 9 - 3 = 6
3. Partner's current age: 20 + 6 = 26
```

---

### 5.3 Tree of Thoughts (ToT)

Explore multiple reasoning paths before committing to one. Best for strategic decisions or problems with no obvious solution path.

**Trigger prompt:**

> _"Generate 3 different solution paths. Evaluate the pros and cons of each. Then pick the best one and execute it."_

---

### 5.4 Self-Consistency

Generate multiple answers to the same question and pick the most consistent or frequent response. Reduces randomness on high-stakes tasks.

**How to use:**

> _"Provide 3 different ways to solve this. Then compare them and return the most reliable answer."_

For automated pipelines: run the prompt 3× via API and select the majority response.

---

### 5.5 ReAct (Reason + Act)

Combine reasoning with tool use (search, API calls, calculator). The model reasons about what it needs, then acts.

**Trigger:**

> _"Reason about the problem, identify what external information you need, then use the available tools to get it before giving your answer."_

---

### 5.6 Retrieval-Augmented Generation (RAG)

Ground the model's answer in provided documents instead of its training data. Eliminates hallucination on domain-specific or real-time questions.

**Trigger:**

> _"Use only the following documents to answer. If the answer is not in the documents, say 'Not found in source.'"_

---

### 5.7 Prompt Chaining

Break complex tasks into a sequence of prompts where the output of Step N becomes the input of Step N+1. More reliable than stuffing everything into one prompt.

**Pipeline example:**

```
Step 1: "Summarize this article in 5 bullet points."
Step 2: "Given these bullet points, identify the 3 biggest risks."
Step 3: "For each risk, write one mitigation strategy in plain English."
```

---

### 5.8 Self-Correction (Iterative / Adversarial)

Ask the model to critique its own output before or after delivery.

- _"Review your previous response for accuracy and conciseness. List 3 ways it could be improved, then rewrite it."_
- _"Find flaws in your own answer before finalizing."_
- _"Does this meet the stated criteria? If not, revise it."_

---

## 6. Constraint Management & Guardrails

### Positive Framing (Critical Rule)

LLMs handle **positive constraints** far better than negative ones.

|❌ Don't Say|✅ Say Instead|
|:--|:--|
|"Don't use jargon."|"Use simple, layman's terms."|
|"Don't write a long essay."|"Limit your response to 50 words."|
|"Don't be rude."|"Maintain a polite and professional tone."|
|"Don't repeat yourself."|"Vary sentence structure. Each point made only once."|

### Anti-Hallucination Tactics

Give the model an explicit "out" when it doesn't know something:

- _"If the answer is not in the provided context, state 'Information not found' rather than speculating."_
- _"If unsure, say 'unknown' and flag your uncertainty."_
- _"Cite the assumptions you are making explicitly."_
- _"List your confidence level (0–100%) for each claim."_

### Aggressive Constraints

Specificity is free. Use it.

- Length: `"≤150 words"` or `"approximately 200 words"`
- Scope: `"Only discuss X. Do not address Y."`
- Style: `"No metaphors. No bullet points. Prose only."`
- Format: `"Output only the code block. No explanation."`

---

## 7. Output Engineering

Always specify what you want back. If you don't define the format, you get the model's default — which is often wrong for your use case.

### JSON — Always provide a schema

```
Output the result as valid JSON with the following keys: 
{ "id": string, "summary": string, "sentiment_score": number (0–1) }
Do not include any text outside the JSON object.
```

### Markdown Tables — Best for comparisons

```
Summarize the differences between Python and Java in a Markdown table 
with columns: Feature, Python, Java.
```

### Code Only

```
Provide only the Python function. No explanation. No preamble. No comments 
unless they are necessary to understand the code.
```

### Structured Text Templates

```
Return a checklist with yes/no criteria.
Format as table with columns: Criterion, Status, Notes.
```

---

## 8. The Iterative Refinement Loop _(This is the real skill)_

A single prompt is a draft. Iteration is how you actually get good outputs.

```
1. GENERATE  → Run your prompt
2. CRITIQUE  → Identify what's wrong: vague? wrong format? off-topic? hallucinated?
3. TIGHTEN   → Fix the specific failure in the prompt
4. REGENERATE → Run again
```

**Meta-prompt for in-session refinement:**

> _"Critique the above answer for inaccuracies, missing details, and format issues. Then rewrite it to fix those problems."_

**Debugging map:**

|Output Problem|Likely Cause|Fix|
|:--|:--|:--|
|Too vague / generic|Instruction not specific enough|Add audience, format, length; use a framework|
|Wrong style or tone|Style not stated|Add "Tone: [X]" and/or provide a style example|
|Hallucinated facts|No grounding, no "out"|Add source text or RAG; add "say I don't know if unsure"|
|Reasoning errors|Model jumped to answer|Add "Think step-by-step" (CoT)|
|Too long|Length not capped|Add `"≤N words"` or `"maximum 3 sentences"`|
|Too short|Depth not requested|Add `"Provide a comprehensive, deep-dive analysis"`|
|Ignores format|Format not shown|Show an example of the desired output structure|
|Repetitive|No constraint against it|Add `"Do not repeat previous points"`|
|Biased|Training data bias|Add `"Provide a balanced view considering multiple perspectives"`|

---

## 9. Model-Specific Tips _(2026 Edition)_

### Anthropic Claude

- **"Contract" style works best:** Structured, bulleted constraints outperform paragraph-style instructions.
- **XML delimiters are native:** Claude is trained to respond well to `<instructions>`, `<context>`, `<constraints>` tags.
- **Long-context tip:** When uploading 100+ page PDFs: _"Referencing page numbers where applicable, answer [Question]."_

### OpenAI (GPT-4 / GPT-5)

- **Reasoning mode:** For math and logic, GPT-5 enters a "thinking" state automatically, but you can prompt it with: _"Enter a deep-reasoning state to solve this edge case."_
- **JSON Mode:** Always use: _"Output must be valid JSON following this schema: [Schema]."_ Never assume JSON will be clean without specifying it.
- **Markdown & triple quotes** are more natural delimiters than XML for GPT models.

### Google Gemini

- **Native multimodality:** Don't describe images — upload them. Ask for spatial reasoning: _"Where is the error in this UI screenshot?"_
- **Override brevity:** Gemini defaults to concise. If you need depth: _"Provide a comprehensive, deep-dive analysis without omitting technical nuances."_

---

## 10. Common Prompting Fallacies _(Interrogate Your Assumptions)_

|Fallacy|What You're Thinking|The Reality|
|:--|:--|:--|
|**The "Please" Fallacy**|Polite language encourages better output|Politeness is token waste. The model responds to **clarity**, not courtesy.|
|**The "More Is Better" Fallacy**|More context = better answers|Long, unstructured context causes the "lost in the middle" effect. Use delimiters and summarization.|
|**The "Magic Word" Fallacy**|"Act as a..." is a silver bullet|Personas only work when backed by **specific behavioral constraints**.|
|**The "Zero-Shot Optimism"**|Complex reasoning works without examples|If a human needs an example, the model definitely does. Use Few-Shot or CoT.|
|**The "One and Done" Fallacy**|First output is the final output|First outputs are drafts. Iteration is the actual skill.|
|**The "Longer = Better" Fallacy**|A 2-page prompt must be more precise|Length without structure is noise. Precision > verbosity, always.|
|**The "Template Collection" Trap**|Hoarding prompt templates = prompt skill|Copying templates without understanding why they work = no improvement loop.|

---

## 11. Power User Techniques

### Reverse Prompting

Give the model an output you like; ask it to reverse-engineer the prompt.

> _"I will provide an output I like. Analyze it and write the prompt that would generate this exact style and structure."_

### Persona Trigger (with guardrails)

> **Good:** _"Act as a strict interviewer evaluating technical answers."_ **Bad:** _"Act as a genius billionaire polymath."_ — adds noise, not signal.

The persona only improves outputs if it is **specific and behaviorally constrained.**

### The "Agnostic" Filter

> _"Provide a platform-agnostic solution that doesn't favor one specific vendor."_

### Recency Bias — Put the Ask Last

Put your core instruction at the **end** of the prompt. The model weights recent tokens more heavily — so the final instruction is more likely to dominate the output.

### Dynamic Variables (for reusable prompts)

Use placeholders to build templates you can reuse:

```
You are writing for [INSERT AUDIENCE].
The topic is [INSERT TOPIC].
Output format: [INSERT FORMAT].
```

### Decomposition for Complex Tasks

Break one vague prompt into a pipeline with explicit steps:

```
Step 1: Summarize the input in 3 bullet points.
Step 2: From those bullets, extract 5 key variables.
Step 3: Analyze the variables for risks.
Step 4: Output a final recommendation in 100 words.
```

---

## 12. LLM-as-a-Judge _(Evaluation)_

For high-stakes or production applications, use a second model pass to evaluate the first output.

**Evaluation prompt template:**

```
You are an impartial judge. Evaluate the following response based on:
1. Accuracy — is it factually correct?
2. Constraint adherence — does it follow all stated rules?
3. Clarity — is it easy to understand?

Score each criterion from 1–10 and provide a one-sentence justification per criterion.
Then give an overall score and a specific suggestion for improvement.

Response to evaluate:
[PASTE OUTPUT HERE]
```

---

## 13. Pre-Mortem — Before You Ship a Prompt

Run this checklist on any prompt you plan to use repeatedly or in production.

> _"What would have to be true for this prompt to fail spectacularly?"_

Common failure modes:

- You copied a template without adapting it → generic outputs
- You didn't measure output quality → no improvement loop
- You relied on one-shot results → unstable, non-reproducible
- You used vague terms like "good," "detailed," or "comprehensive" without defining them
- You assumed the model knows your context → it doesn't

---

## 14. The Mechanism-Based Framework _(For Those Who Want to Understand Why)_

Every technique maps to a mechanism inside the model. Understanding this makes you better at debugging failures.

|Principle|Mechanism|Tactical Execution|
|:--|:--|:--|
|**Constraint > Instruction**|Reduces the probability search space|"Write Python. No imports outside `pandas`. Max 50 lines."|
|**Few-Shot > Zero-Shot**|Primes attention via pattern matching|3 input/output examples before the real request|
|**Chain of Thought**|Forces sequential token generation|"Think step-by-step. Output reasoning before final answer."|
|**Role Priming**|Biases latent space toward specific vocabulary|"Act as a Senior Systems Architect. Critique this design."|
|**Delimiter Usage**|Separates context from instruction in attention|Use `"""`, `###`, or `<tags>`|
|**Positive Constraints**|Avoids negation confusion in token space|"Use plain English" not "Don't use jargon"|

---

## 15. Master Checklist _(Run Before Sending)_

```
[ ] Role defined with specific behavioral constraints?
[ ] Context provided with clear delimiters?
[ ] Objective stated precisely — no vague verbs like "improve" or "optimize"?
[ ] Output format specified (JSON schema / Markdown table / prose / code only)?
[ ] Negative constraints rewritten as positive framing?
[ ] Anti-hallucination "out" provided ("say I don't know if unsure")?
[ ] Reasoning steps requested for complex logic (CoT)?
[ ] Examples provided if format or style must be exact (Few-Shot)?
[ ] Length constrained explicitly?
[ ] Core instruction placed at the end of the prompt (recency bias)?
[ ] Is prompting even the right tool here, or should this be structured differently?
```

---

## Quick Reference: Common Use Cases

| Goal                  | Prompt Pattern                                                                 |
| :-------------------- | :----------------------------------------------------------------------------- |
| Summarization         | "Summarize in 3 bullet points. Focus on key takeaways for [AUDIENCE]."         |
| Brainstorming         | "List 10 [X]. Prioritize by [criterion]."                                      |
| Code generation       | "Write a Python function to [task]. Handle [edge cases]. No external imports." |
| Data analysis         | "Analyze this dataset for [pattern]. Output findings as a Markdown table."     |
| Critique / review     | "Evaluate strengths and weaknesses of [X]. Be specific, not general."          |
| Translation           | "Translate to [language]. Preserve [tone / formality level]."                  |
| Classification        | "Classify this as [A], [B], or [C]. Return only the label, no explanation."    |
| Roleplay / simulation | "You are [role]. I am [role]. We are discussing [topic]. Begin."               |
| Comparison            | "Compare [A] vs [B] in a Markdown table with columns: Feature, A, B, Verdict." |
|                       |                                                                                |

---

## Further Reading

- DeepLearning.AI — [_ChatGPT Prompt Engineering for Developers_ (course)]https://www.deeplearning.ai/short-courses/chatgpt-prompt-engineering-for-developers/]
- Anthropic — [_Prompt Engineering Overview_](docs.anthropic.com)
- OpenAI — [_Prompt Engineering Guide_](platform.openai.com/docs)
- GitHub — See [prompts.chat](https://github.com/f/prompts.chat) for an open-source prompt library
- Tools: [Prompt Framework Builder](https://kungpowdragon.github.io/AI-Prompt-Framework-Builder/)
