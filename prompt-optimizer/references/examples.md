# Prompt Optimization Examples

This document provides before-and-after examples of prompt optimization across different use cases.

## Example 1: Code Review Request

### Before (Poor Quality)
```
Review my code
```

**Problems:**
- No context about what type of review is needed
- No information about the code's purpose
- No guidance on what to focus on
- Missing any quality criteria

### After (Optimized)
```
Review this Python authentication module for a web application. Focus on:

1. Security vulnerabilities (SQL injection, XSS, improper session handling)
2. Code organization and maintainability
3. Error handling completeness
4. Performance considerations for high-traffic scenarios

The code will be used in production serving 100k+ daily users. Prioritize security issues over style concerns.

If any security issue is found, explain the potential impact and provide a secure alternative implementation.
```

**Improvements:**
- Specifies code type and purpose
- Lists explicit review criteria
- Provides usage context
- Sets clear priorities
- Defines expected output detail

---

## Example 2: Content Creation

### Before (Poor Quality)
```
Write a blog post about AI
```

**Problems:**
- Topic too broad
- No target audience specified
- Missing tone/style guidance
- No length constraints
- Unclear purpose or angle

### After (Optimized)
```
Write a 1,200-word blog post about practical applications of AI in small business operations.

Target audience: Small business owners (10-50 employees) with limited technical background who are curious about AI but skeptical about ROI.

Tone: Encouraging but realistic, conversational, no hype

Structure:
1. Opening hook: Common small business challenge
2. 3-4 specific AI use cases with real cost/time savings
3. "Getting started" section with concrete first steps
4. Address common concerns (cost, complexity, data privacy)
5. Closing: Balanced perspective on AI adoption

Include at least one concrete example with specific numbers. Avoid overly technical jargon. If technical terms are necessary, explain them simply.
```

**Improvements:**
- Specific topic scope
- Defined target audience
- Clear tone expectations
- Specified structure and length
- Content requirements (examples, numbers)
- Style guidelines

---

## Example 3: Data Analysis

### Before (Poor Quality)
```
Analyze this sales data
```

**Problems:**
- No analysis objective
- Missing key questions to answer
- No output format specified
- Unclear what insights are valuable

### After (Optimized)
```
Analyze the attached Q4 sales data to identify growth opportunities for Q1 planning.

Key questions to answer:
1. Which product categories showed strongest/weakest growth?
2. Are there regional performance patterns?
3. What seasonal trends are evident?
4. Which customer segments are growing/shrinking?

Context: This analysis will inform Q1 budget allocation across product lines and regions.

Output format:
- Executive summary (3-4 key findings)
- Detailed findings for each question with supporting data
- 3-5 actionable recommendations prioritized by potential impact

If the data is insufficient to answer any question confidently, state what additional data would be needed.
```

**Improvements:**
- Clear analysis objective
- Specific questions to answer
- Business context provided
- Structured output format
- Permits acknowledging data limitations

---

## Example 4: Technical Documentation

### Before (Poor Quality)
```
Document this API
```

**Problems:**
- No audience specified
- Missing documentation scope
- Unclear what level of detail is needed
- No format preferences

### After (Optimized)
```
Create API documentation for the User Management endpoints intended for external third-party developers integrating with our platform.

Audience: Developers with general REST API experience but unfamiliar with our system.

For each endpoint include:
1. Endpoint URL and HTTP method
2. Purpose and use case
3. Authentication requirements
4. Request parameters (with types, required/optional, validation rules)
5. Example request with curl
6. Example successful response
7. Possible error responses with status codes
8. Rate limiting information

Organization: Group endpoints by resource (Users, Permissions, Sessions)

Tone: Professional but friendly, assume the reader is competent but unfamiliar with our specific implementation.

Include a "Quick Start" section at the beginning with a complete authentication and first API call example.
```

**Improvements:**
- Defined target audience
- Explicit scope and structure
- Detailed content requirements
- Clear organization principle
- Tone specification
- Additional helpful section requested

---

## Example 5: Problem Solving

### Before (Poor Quality)
```
My app is slow. Fix it.
```

**Problems:**
- No diagnostic information
- Unclear what "slow" means
- Missing context about the application
- No information about what's been tried

### After (Optimized)
```
Help me diagnose and resolve performance issues in a React web application.

Symptoms:
- Initial page load takes 8-12 seconds (target: under 3 seconds)
- Scrolling feels janky/unresponsive
- Issue affects all users, worse on mobile devices

App context:
- Single-page React app with Redux state management
- Displays data dashboards with charts (using Recharts)
- Fetches data from REST API (responses typically 200-500kb)
- Hosted on AWS CloudFront + S3

What I've tried:
- Checked network tab: API responses are fast (< 500ms)
- Main bundle size is 2.1 MB
- Haven't implemented code splitting yet

Please:
1. Identify likely performance bottlenecks based on symptoms
2. Suggest specific diagnostic steps to confirm root causes
3. Provide prioritized optimization recommendations
4. For top 2-3 recommendations, include example implementation

If you need additional diagnostic information to provide better recommendations, please specify what to check.
```

**Improvements:**
- Quantified the problem ("8-12 seconds")
- Provided relevant technical context
- Listed what's been investigated
- Structured the request with clear steps
- Permits requests for more information

---

## Example 6: Creative Writing

### Before (Poor Quality)
```
Write a story
```

**Problems:**
- No genre, theme, or setting
- Missing tone/mood guidance
- No length specification
- Unclear target audience
- No creative constraints or requirements

### After (Optimized)
```
Write a 1,500-word science fiction short story for adult readers.

Setting: Near-future Earth (2045) where climate change has forced mass migration to underground cities.

Core concept: A maintenance worker discovers an old seed vault and must decide whether to reveal it to the authoritarian government or protect it for a potential resistance movement.

Tone: Thoughtful and character-driven rather than action-focused; bittersweet rather than purely dystopian

Requirements:
- Third-person limited perspective following the protagonist
- Include sensory details about the underground environment
- Show the protagonist's internal conflict, not just external action
- Ending should be ambiguous rather than fully resolved

Avoid:
- Excessive technical jargon
- Obvious "chosen one" tropes
- Overly explanatory world-building dumps

Focus on: Character psychology, moral ambiguity, and atmospheric world-building
```

**Improvements:**
- Specific genre and audience
- Clear setting and premise
- Defined tone and mood
- Structural requirements
- Creative constraints and preferences
- Guidance on what to emphasize

---

## Example 7: Learning/Education

### Before (Poor Quality)
```
Teach me Python
```

**Problems:**
- Scope impossibly broad
- No information about current skill level
- Missing learning goals
- No time constraints or format preference

### After (Optimized)
```
Create a focused Python lesson on list comprehensions for someone who understands basic Python syntax (variables, loops, conditionals, lists) but hasn't used list comprehensions.

Learning objectives:
1. Understand list comprehension syntax and structure
2. Convert simple for loops to list comprehensions
3. Know when list comprehensions improve code readability
4. Recognize when NOT to use list comprehensions

Format:
1. Brief concept explanation (2-3 sentences)
2. Simple example with equivalent for-loop comparison
3. 3-4 progressively complex examples (filtering, transformation, nested)
4. 3 practice problems with solutions
5. "When to use / when to avoid" guidelines

Teaching approach: Show practical examples first, then explain the pattern. Use relatable scenarios (processing shopping lists, filtering student grades, etc.) rather than abstract foo/bar examples.

Keep total lesson under 1,000 words so it can be completed in one focused session.
```

**Improvements:**
- Narrow, specific topic
- Current skill level stated
- Clear learning objectives
- Structured lesson format
- Teaching methodology specified
- Practical length constraint

---

## Example 8: Research Synthesis

### Before (Poor Quality)
```
Summarize these articles
```

**Problems:**
- No synthesis goal or angle
- Missing target length
- Unclear what aspects to focus on
- No guidance on handling contradictions

### After (Optimized)
```
Synthesize findings from the attached 5 research papers on remote work productivity to answer: "What factors most significantly impact productivity in remote work environments?"

Focus areas:
1. Points of consensus across studies
2. Significant contradictions or disagreements
3. Methodological differences that might explain contradictions
4. Practical implications for managers

Output structure:
1. Overview paragraph: Main consensus findings (if any)
2. Key factors section: Organize by factor (communication, environment, autonomy, etc.), noting level of agreement
3. Contradictions section: Highlight disagreements with possible explanations
4. Research gaps: What questions remain unanswered?
5. Practical recommendations: 3-5 evidence-based suggestions

Length: 1,200-1,500 words

If studies use different definitions of "productivity," note this explicitly and explain how it affects comparisons.
```

**Improvements:**
- Clear synthesis goal (answer specific question)
- Defined focus areas
- Structured output format
- Length specification
- Guidance on handling complexity and contradictions

---

## Pattern Summary

Across all examples, effective prompts consistently include:

1. **Specific scope and objective** - Not "analyze data" but "analyze Q4 sales data to identify growth opportunities"

2. **Context about purpose** - Why this is needed, how it will be used

3. **Target audience** - Who will consume this output

4. **Explicit structure** - How to organize the response

5. **Quality criteria** - What makes a good response

6. **Constraints** - Length, tone, format, what to avoid

7. **Permission for uncertainty** - When to say "I don't know" or ask for more info

8. **Current state information** - What's known, what's been tried, current skill level
