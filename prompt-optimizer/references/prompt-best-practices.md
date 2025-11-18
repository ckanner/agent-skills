# Prompt Engineering Best Practices

This reference document contains comprehensive best practices for crafting effective prompts for AI models like Claude.

## Core Principles

### 1. Be Clear and Direct

**Principle:** Tell the model explicitly what is wanted, without assuming it will infer the intent.

**Why it matters:** Modern AI models follow instructions literally. Vague requests lead to generic or misaligned outputs.

**Examples:**

**Poor:** "Create an analytics dashboard"
- Problem: No clarity on scope, features, or requirements

**Better:** "Create an analytics dashboard. Include as many relevant features and interactions as possible"
- Improvement: Specifies scope and expectations

**Poor:** "Write a summary"
- Problem: Unclear about length, focus, or audience

**Better:** "Write a 3-paragraph executive summary highlighting key financial metrics and strategic recommendations for C-level executives"
- Improvement: Defines length, focus, and audience

### 2. Provide Context and Motivation

**Principle:** Explain WHY certain requirements matter to help the AI understand the underlying goal.

**Why it matters:** Context helps the AI make better decisions when facing ambiguous situations or trade-offs.

**Examples:**

**Less effective:** "NEVER use bullet points"
- Problem: Sounds arbitrary, AI doesn't understand the reasoning

**More effective:** "I prefer responses in natural paragraph form rather than bullet points because I find flowing prose easier to read and more suitable for formal reports"
- Improvement: Explains the reasoning, helps AI understand when to apply this preference

**Less effective:** "Use simple language"
- Problem: Vague definition of "simple"

**More effective:** "Use simple language suitable for a general audience without technical background, as this content will be published in a consumer-facing blog"
- Improvement: Defines audience and purpose

### 3. Be Specific with Requirements

**Principle:** Provide concrete details about constraints, requirements, and desired outcomes.

**Why it matters:** Specificity reduces ambiguity and ensures outputs match expectations.

**Examples:**

**Vague:** "Create a meal plan for a Mediterranean diet"
- Problem: No constraints on calories, dietary restrictions, or goals

**Specific:** "Design a Mediterranean diet meal plan for pre-diabetic management. 1,800 calories daily, emphasis on low glycemic foods, avoid processed sugars, include 5-6 small meals throughout the day"
- Improvement: Defines medical context, caloric target, food restrictions, and meal frequency

**Vague:** "Analyze this code for issues"
- Problem: Unclear what types of issues to focus on

**Specific:** "Analyze this Python code for security vulnerabilities, focusing on SQL injection risks, XSS vulnerabilities, and improper input validation. Also check for performance bottlenecks in database queries"
- Improvement: Specifies analysis focus areas

### 4. Use Examples (Few-Shot Learning)

**Principle:** Show concrete examples of desired input-output patterns.

**Why it matters:** Modern models like Claude 4.x pay close attention to examples, often more than written instructions.

**Structure:**
```
[Task description]

Example 1:
Input: [example input]
Output: [example output]

Example 2:
Input: [example input]
Output: [example output]

Now process:
Input: [actual input]
```

**Use cases:**
- Defining output format
- Establishing tone and style
- Showing edge case handling
- Demonstrating complex transformations

### 5. Allow Expression of Uncertainty

**Principle:** Explicitly permit the AI to say "I don't know" rather than guessing or fabricating information.

**Why it matters:** Prevents hallucination and builds trust through honesty.

**Examples:**

**Standard prompt:** "What was the Q4 revenue for Company X?"
- Problem: AI may fabricate numbers if data is unavailable

**Better prompt:** "What was the Q4 revenue for Company X? If the data is insufficient to draw conclusions, say so rather than speculating"
- Improvement: Explicitly permits uncertainty

**Standard prompt:** "Analyze this medical report"
- Problem: AI may overreach its capabilities

**Better prompt:** "Analyze this medical report. Focus on factual observations from the data. If any medical interpretation requires expertise beyond pattern recognition, clearly state that and recommend consulting a medical professional"
- Improvement: Sets boundaries and encourages appropriate caution

## Advanced Techniques

### 1. Prefill (Response Priming)

**Technique:** Start the AI's response to guide format, tone, or structure.

**Use cases:**
- Forcing specific output formats (JSON, XML)
- Eliminating preamble text
- Establishing tone immediately
- Starting with specific phrases

**Example - JSON Output:**
```
User: Extract the name and price from this product description into JSON.
Assistant: {
```

Result: The AI continues directly with JSON, no preamble.

**Example - Direct Answer:**
```
User: What's the capital of France?
Assistant: The capital is
```

Result: AI completes with "Paris" instead of adding "The capital of France is Paris"

### 2. Chain of Thought (Step-by-Step Reasoning)

**Technique:** Request explicit step-by-step reasoning before the final answer.

**Why it matters:** Improves accuracy on complex reasoning tasks, makes logic transparent.

**Three approaches:**

**a) Basic Chain of Thought:**
```
Solve this problem step-by-step: [problem]
```

**b) Guided Chain of Thought:**
```
Analyze this contract clause. Follow these reasoning stages:
1. Identify the key obligations
2. Note any ambiguous language
3. Assess potential risks
4. Provide final interpretation
```

**c) Structured Chain of Thought:**
```
Solve this problem. Use this format:

<reasoning>
[Your step-by-step thinking]
</reasoning>

<answer>
[Your final answer]
</answer>
```

### 3. Control Output Format

**Principle:** Explicitly specify the desired output structure.

**Guidelines:**
- Tell the AI what TO do, not what NOT to do
- Match prompt style to desired output style
- Use prefilling for strict format requirements
- Provide format examples

**Examples:**

**For structured data:**
```
Extract key information in this JSON format:
{
  "company": "string",
  "revenue": "number with currency",
  "year": "YYYY"
}
```

**For analytical reports:**
```
Provide analysis in this structure:
1. Executive Summary (2-3 sentences)
2. Key Findings (bullet points)
3. Detailed Analysis (paragraphs)
4. Recommendations (numbered list)
```

### 4. Prompt Chaining

**Technique:** Break complex tasks into sequential prompts where each builds on previous outputs.

**Why it matters:**
- Improves quality by focusing on one aspect at a time
- Allows intermediate verification
- Enables iterative refinement

**Example - Research Summary:**

**Prompt 1:** "Summarize the key findings from this 50-page research paper"
**Prompt 2:** "Review this summary for accuracy. Identify any misrepresentations or missing critical points"
**Prompt 3:** "Based on the review feedback, create an improved summary"

**Example - Code Review:**

**Prompt 1:** "Analyze this code for security vulnerabilities"
**Prompt 2:** "Now review the same code for performance issues"
**Prompt 3:** "Based on both analyses, prioritize the issues and suggest an implementation order"

## Common Patterns to Avoid

### 1. Over-Constraining with Roles

**Problem:** Overly specific role definitions can limit helpfulness.

**Poor:** "You are a Senior Java Enterprise Architect with exactly 15 years of experience who only uses Spring Boot and refuses to discuss other frameworks"
- Too rigid, may prevent useful suggestions

**Better:** "Provide architectural guidance for a Java Spring Boot application, focusing on enterprise-scale best practices"
- Provides context without unnecessary constraints

### 2. Assuming Mind-Reading

**Problem:** Leaving requirements implicit rather than explicit.

**Poor:** "Make it better"
- AI doesn't know what "better" means in this context

**Better:** "Improve code readability by: adding descriptive variable names, extracting complex logic into well-named functions, and adding explanatory comments for non-obvious algorithms"
- Explicitly defines improvement criteria

### 3. Negative Instructions

**Problem:** Telling AI what NOT to do is less effective than saying what TO do.

**Less effective:** "Don't be too technical, don't use jargon, don't make it too long"

**More effective:** "Use conversational language suitable for a general audience, explain concepts with everyday analogies, keep the response under 300 words"

### 4. Over-Engineering

**Problem:** Using all advanced techniques when simple clarity would suffice.

**When to use simple prompts:** Single-step tasks, straightforward requests, well-defined operations

**When to use advanced techniques:** Multi-step reasoning, complex analysis, tasks requiring specific formats, ambiguous requirements

## Troubleshooting Guide

### Response is too generic
**Solutions:**
- Add specific constraints and requirements
- Provide concrete examples
- Include context about the use case
- Specify the audience and purpose

### AI goes off-topic
**Solutions:**
- State the objective more clearly at the beginning
- Provide more context about the goal
- Break complex requests into smaller prompts
- Use structured output format

### Format is inconsistent
**Solutions:**
- Add explicit format examples
- Use prefilling to start the response
- Provide a template or schema
- Use structured tags (e.g., `<answer>`, `<reasoning>`)

### Includes unnecessary preamble
**Solutions:**
- Use prefilling to skip straight to content
- Add explicit instruction: "Begin directly with [content type], no introduction"
- Provide example that starts immediately with content

### Task is too complex
**Solutions:**
- Use prompt chaining to break into steps
- Apply chain of thought reasoning
- Create a multi-stage workflow
- Simplify the request scope

### Contains fabricated information
**Solutions:**
- Explicitly permit expressions of uncertainty
- Request citations or source attribution
- Ask for confidence levels
- Request "I don't know" when data is insufficient

## Quality Checklist

Before finalizing a prompt, verify:

- [ ] **Clarity:** Is the request unambiguous?
- [ ] **Specificity:** Are constraints and requirements concrete?
- [ ] **Context:** Does the AI understand WHY this matters?
- [ ] **Examples:** Are there examples for complex or ambiguous requirements?
- [ ] **Format:** Is the desired output structure clear?
- [ ] **Uncertainty:** Is the AI permitted to express limitations?
- [ ] **Scope:** Is the task appropriately scoped (not too complex)?
- [ ] **Tone:** Is the desired communication style specified?
- [ ] **Success criteria:** Is it clear what constitutes a good output?

## Decision Framework

Use this flowchart to select appropriate techniques:

1. **Is the request clear and specific?**
   - No → Add clarity, specificity, context
   - Yes → Continue

2. **Is the task simple and straightforward?**
   - Yes → Use basic clear prompt
   - No → Continue

3. **Is a specific format required?**
   - Yes → Add format specification or prefilling
   - No → Continue

4. **Is the task complex or multi-faceted?**
   - Yes → Consider prompt chaining
   - No → Continue

5. **Does it require reasoning or analysis?**
   - Yes → Use chain of thought
   - No → Use basic prompt

6. **Are there ambiguous or nuanced requirements?**
   - Yes → Add examples (few-shot)
   - No → Proceed with current prompt
