---
description: Start a new deep research session with systematic multi-phase workflow
allowed-tools:
  - Read
  - Write
  - Bash
  - TodoWrite
  - WebSearch
  - WebFetch
  - Task
---

# Start Deep Research Session

Begin deep research for: $ARGUMENTS

## Process Overview
This command creates a structured research session with 4 phases:
1. **Planning**: Break question into 5-7 sub-questions
2. **Gathering**: Collect 15-25 high-quality sources  
3. **Analysis**: Synthesize findings from sources
4. **Report**: Generate interactive HTML report with citations

## Implementation

### 1. Validate Input
```
RESEARCH_QUESTION = "$ARGUMENTS"
if empty(RESEARCH_QUESTION):
    error("Please provide a research question")
    suggest("Usage: /research-start [your research question]")
    exit
```

### 2. Generate Session ID
```
TIMESTAMP = current_datetime("YYYY-MM-DD-HHMM")
TOPIC_SLUG = create_slug(RESEARCH_QUESTION)  // 3-5 key words, lowercase, hyphenated
SESSION_ID = TIMESTAMP + "-" + TOPIC_SLUG
```

### 3. Check for Active Session
```
ACTIVE_SESSION_FILE = ".deep-research/.current-research"
if file_exists(ACTIVE_SESSION_FILE):
    ACTIVE_SESSION = read_file(ACTIVE_SESSION_FILE).strip()
    if not empty(ACTIVE_SESSION):
        display("Active session found: " + ACTIVE_SESSION)
        CHOICE = prompt("Continue existing session or start new?", ["continue", "new"], "continue")
        if CHOICE == "continue":
            redirect_to_research_status()
            exit
```

### 4. Create Session Structure
```
SESSION_DIR = ".deep-research/sessions/" + SESSION_ID
create_directory(".deep-research")
create_directory(SESSION_DIR)
create_directory("sources")
```

### 5. Initialize Metadata
```
METADATA = {
    "id": SESSION_ID,
    "question": RESEARCH_QUESTION,
    "started": current_iso_timestamp(),
    "phase": "planning",
    "status": "active",
    "subQuestions": 0,
    "sourcesCollected": 0,
    "files": {
        "plan": SESSION_ID + "-plan.md",
        "findings": SESSION_ID + "-findings.md", 
        "report": SESSION_ID + "-report.html"
    }
}
write_json(SESSION_DIR + "/metadata.json", METADATA)
```

### 6. Optional Clarifying Questions
```
display("I can proceed with smart defaults or customize the approach:")
SETUP_MODE = prompt("Setup mode?", ["quick", "custom"], "quick")

if SETUP_MODE == "custom":
    Q1 = prompt("Time focus?", ["recent", "all"], "recent")
    Q2 = prompt("Depth level?", ["general", "expert"], "general") 
    Q3 = prompt("Report format?", ["html", "markdown"], "html")
    USER_PREFERENCES = {timeframe: Q1, depth: Q2, format: Q3}
else:
    USER_PREFERENCES = {timeframe: "recent", depth: "general", format: "html"}
```

### 7. Execute Research Planning
```
Transform into expert research strategist.
Create comprehensive research plan for: RESEARCH_QUESTION
Incorporate user preferences: USER_PREFERENCES

Generate 5-7 sub-questions that:
- Are specific and answerable
- Cover different aspects of the main question
- Include search strategies and source types
- Have priority levels (High/Medium/Low)

Structure as detailed research plan with:
- Overview of research scope
- Numbered sub-questions with specifications
- Research sequence and expected outcomes
- Synthesis approach
```

### 8. Save Plan and Update State
```
write_file(SESSION_ID + "-plan.md", RESEARCH_PLAN)
write_file(".deep-research/.current-research", SESSION_ID)
update_metadata(SESSION_DIR, {subQuestions: count_subquestions(RESEARCH_PLAN)})
```

### 9. Initialize Progress Tracking
```
create_todo([
    {content: "Research Planning", status: "completed", priority: "high"},
    {content: "Information Gathering", status: "pending", priority: "high"},
    {content: "Analysis & Synthesis", status: "pending", priority: "medium"},
    {content: "Report Generation", status: "pending", priority: "medium"}
])
```

### 10. Request Approval
```
display_plan_summary(SESSION_ID, RESEARCH_PLAN)
APPROVAL = prompt("Approve plan and proceed to information gathering?", ["yes", "modify"], "yes")
if APPROVAL == "yes":
    display("✅ Research session created: " + SESSION_ID)
    display("📝 Plan saved: " + SESSION_ID + "-plan.md")
    display("▶️  Use /research-status to begin information gathering")
else:
    display("Plan saved for review. Use /research-status to continue when ready.")
```

## Research Plan Template

```markdown
# Research Plan: [TITLE]

**Session ID**: [SESSION_ID]
**Date**: [DATE]
**Question**: [RESEARCH_QUESTION]

## Overview
[2-3 sentences explaining research scope and objectives]

## Sub-Questions

### 1. [Sub-Question]
- **Question**: [Specific question]
- **Search Strategy**: [Keywords and approach]
- **Source Types**: [Academic, industry, news, etc.]
- **Priority**: [High/Medium/Low]

### 2. [Sub-Question]
[Same structure...]

[Continue for 5-7 sub-questions]

## Research Sequence
1. Start with: [Priority question and rationale]
2. Then: [Next priority]
[etc.]

## Expected Outcomes
- Q1: [Expected findings]
- Q2: [Expected findings]
[etc.]

## Synthesis Approach
[How findings will be combined to answer main question]
```

## Metadata Structure

```json
{
  "id": "YYYY-MM-DD-HHMM-topic-slug",
  "question": "Original research question",
  "started": "ISO timestamp",
  "phase": "planning|gathering|analysis|report",
  "status": "active|completed|paused", 
  "subQuestions": 0,
  "sourcesCollected": 0,
  "files": {
    "plan": "session-plan.md",
    "findings": "session-findings.md",
    "report": "session-report.html"
  }
}
```

## Implementation Notes

### Session ID Generation
- Extract 3-5 key words from research question
- Convert to lowercase, hyphenated format
- Examples:
  - "AI impact on productivity" → "ai-impact-productivity"
  - "Climate change solutions" → "climate-change-solutions"

### Directory Structure
```
./
├── .deep-research/
│   ├── .current-research
│   └── sessions/[session-id]/metadata.json
├── sources/
├── [session-id]-plan.md
├── [session-id]-findings.md
└── [session-id]-report.html
```

### Planning Phase Requirements
- 5-7 well-formed sub-questions
- Specific search strategies for each question
- Source type identification
- Priority assignment
- Clear synthesis approach

### Success Indicators
- Session directory created successfully
- Metadata.json saved with valid structure
- Research plan saved as [session-id]-plan.md
- Global state updated in .current-research
- User approval obtained or plan saved for review