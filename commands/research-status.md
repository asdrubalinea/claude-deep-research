---
description: Check current research session progress and continue workflow from where you left off
allowed-tools:
  - Read
  - Write
  - Bash
  - TodoWrite
  - WebSearch
  - WebFetch
  - Task
---

# Check Research Status

Show current research session progress and continue workflow.

## Description

This command displays the status of your active research session, shows current progress through the four phases, and continues from where you left off. It provides a clear view of completed work and next steps.

## Instructions

1. **Check for Active Session**
   - Read `.deep-research/.current-research` file in user's current directory
   - If file doesn't exist or is empty:
     - Show message: "No active research session"
     - Suggest using `/research-start` to begin new research
     - List available research sessions with `/research-list`
     - Exit

2. **Load Session Data**
   - Parse `.deep-research/.current-research` to get session ID
   - Construct session metadata path as `.deep-research/sessions/[SESSION_ID]/metadata.json`
   - Read `metadata.json` from `.deep-research/sessions/[SESSION_ID]/` for current phase and progress in parallel
   - If metadata.json doesn't exist, create it with default values:
     ```json
     {
       "title": "[extracted from directory name]",
       "created": "[session start timestamp]",
       "last_updated": "[current timestamp]",
       "phase": 1,
       "phase_name": "Planning",
       "progress": {
         "planning_complete": false,
         "gathering_complete": false,
         "analysis_complete": false,
         "report_complete": false
       },
       "current_action": "review_research_plan",
       "next_steps": ["Review and approve research plan", "Proceed to Phase 2: Information Gathering"],
       "completed_actions": [],
       "total_sources": 0,
       "findings_count": 0
     }
     ```

3. **Display Formatted Status**
   - Show current session information with progress indicators
   - Display recent completed actions
   - Show current phase and next steps
   - Include session statistics and timing information

4. **Determine Continuation Point**
   - Based on phase and progress, identify where to resume
   - Load appropriate files and continue workflow
   - Update metadata with continuation timestamp

## Status Display Format

```
🔬 Active Research Session: [Session Title]
Started: [X hours/days ago] | Last updated: [X minutes/hours ago]
Phase: [X/4] [Phase Name] | Progress: [Status indicators]

📊 Session Statistics:
• Sources collected: [X]
• Findings documented: [X]
• Current action: [Action description]

✅ Recent Progress:
• [Most recent completed action with timestamp]
• [Second most recent action with timestamp]
• [Third most recent action with timestamp]

🎯 Current Phase: [Phase Name]
Status: [Phase-specific status description]

⏭️ Next Steps:
[Numbered list of immediate next actions]

───────────────────────────────────────────────────────────

[Phase-specific continuation prompt or question]
```

### Phase-Specific Status Indicators

**Phase 1 - Planning:**
```
Progress: 📋 Planning [⏳ In Progress | ✅ Complete]
```

**Phase 2 - Information Gathering:**
```
Progress: 📋 Planning ✅ | 🔍 Gathering [⏳ In Progress | ✅ Complete]
Sources: [X/target] collected
```

**Phase 3 - Analysis:**
```
Progress: 📋 Planning ✅ | 🔍 Gathering ✅ | 📊 Analysis [⏳ In Progress | ✅ Complete]
Findings: [X] documented
```

**Phase 4 - Report Generation:**
```
Progress: 📋 Planning ✅ | 🔍 Gathering ✅ | 📊 Analysis ✅ | 📄 Report [⏳ In Progress | ✅ Complete]
```

**All Phases Complete:**
```
Progress: 📋 Planning ✅ | 🔍 Gathering ✅ | 📊 Analysis ✅ | 📄 Report ✅
Status: 🎉 Research Complete
```

## Continuation Flow

### Phase 1 - Planning
- **If planning incomplete:**
  - Load `[SESSION_ID]-plan.md` if exists
  - Show research plan review prompt
  - Ask for approval to proceed to Phase 2
  - Update metadata on approval

- **If planning complete:**
  - Show plan summary
  - Ask Phase 2 clarifying questions before transitioning
  - Transition to Phase 2 after questions answered

### Phase 2 - Information Gathering
- **If gathering in progress:**
  - Load `sources/source-inventory.md` if exists
  - Show current source count and gaps
  - Continue with next sub-question from research plan
  - ## ultrathink
    Information gathering strategy requires careful evaluation of source quality, relevance, and coverage to ensure comprehensive research while maintaining efficiency and avoiding redundancy.
    
    Transform into meticulous research assistant specializing in systematic information gathering.
    Your role is to collect comprehensive, high-quality sources based on the approved research plan.
    
    For ALL sub-questions in the research plan:
    
    A. Parallel Search Phase - Execute searches for multiple sub-questions simultaneously:
    [Use multiple tool calls in a single response - up to 5 parallel searches]
    - Prioritize recent content (within last 2-3 years unless historical context needed)
    - Authoritative sources (edu, gov, established publications)
    - Diverse perspectives (academic, industry, news, expert opinions)
    - Open access content (avoid paywalls when possible)
    
    B. Parallel Content Extraction - Fetch content in parallel:
    [Use multiple WebFetch calls simultaneously - up to 5 at once]
    - Extract all relevant information about specific sub-questions
    - Include statistics, quotes, examples, publication date and author
    - Focus on specific aspects for each sub-question
    - Extract methodology and key findings
    - Note limitations and bias indicators
    
    C. Save Sources with descriptive filenames:
    Format: [source-number]-[domain]-[brief-title].md
    Example: 01-mit-edu-ai-coding-productivity-study.md
    
    Each source should include:
    - Full title, URL, dates, author, type
    - Relevance to sub-questions
    - Full extracted content
    - 2-3 bullet points of key information
    
    Search Best Practices:
    - Use quotation marks for exact phrases
    - Combine related terms with OR/AND operators
    - Add site restrictions: site:edu OR site:gov
    - Exclude unwanted results: -advertisement -sponsored
    - Use date filters when relevant
    
    Quality Indicators:
    - Citations and references
    - Author credentials
    - Publication reputation
    - Data with sources
    - Balanced perspective
    - Recent updates
  - Present parallel WebSearch and WebFetch prompts for next sources (up to 5 concurrent)

- **If gathering complete:**
  - Show source collection summary
  - Ask Phase 3 clarifying questions before transitioning
  - Transition to Phase 3 after questions answered

### Phase 3 - Analysis
- **If analysis in progress:**
  - Load all sources from `sources/` directory in parallel
  - Show current findings progress
  - Continue analysis from last documented finding
  - ## ultrathink
    Source analysis and synthesis requires deep analytical thinking to identify patterns, reconcile contradictions, extract meaningful insights, and construct coherent arguments from disparate information sources.
    
    Transform into expert research analyst specializing in synthesizing complex information from multiple sources.
    Your role is to transform raw research materials into coherent, insightful analysis that addresses the original research questions.
    
    For Each Sub-Question:
    
    1. Extract Key Findings
    - Identify 3-5 most relevant findings per source
    - Note specific data points, statistics, or expert quotes
    - Distinguish between facts, opinions, and speculation
    - Record the source file for each finding
    
    2. Identify Patterns and Themes
    - Look for recurring concepts across sources
    - Note consensus views vs. divergent opinions
    - Identify emerging trends or shifts over time
    - Map relationships between different findings
    
    3. Detect Contradictions and Gaps
    - Flag conflicting information between sources
    - Note missing information or unanswered aspects
    - Identify potential biases or limitations in sources
    - Record areas needing further investigation
    
    4. Assess Source Quality
    - Consider recency and relevance of each source
    - Note authority and expertise of authors
    - Evaluate methodology where applicable
    - Flag any concerns about reliability
    
    Synthesis Process:
    - Cross-reference findings across sub-questions
    - Identify overarching themes
    - Build coherent narrative
    - Move beyond summarization to interpretation
    - Draw meaningful conclusions from evidence
    - Maintain academic rigor with citations
    - Present balanced, objective analysis
  - Present analysis prompts for remaining sources

- **If analysis complete:**
  - Show findings summary
  - Ask Phase 4 clarifying questions before transitioning
  - Transition to Phase 4 after questions answered

### Phase 4 - Report Generation
- **If report in progress:**
  - Load `[SESSION_ID]-findings.md` and source data
  - Show current report status
  - Continue report generation from last section
  - ## ultrathink
    Report generation requires careful structuring to transform analytical findings into a polished, accessible, and visually appealing presentation that effectively communicates research insights.
    
    Transform into professional report writer specializing in creating comprehensive, interactive research reports.
    Your role is to transform analytical findings into a polished HTML report using Claude Artifacts.
    
    Content Structure:
    1. Title Page - Research topic, date, executive summary, table of contents
    2. Introduction - Context, objectives, methodology, scope
    3. Key Findings - Organized by research question with visual hierarchy
    4. Detailed Analysis - Comprehensive coverage with citations
    5. Conclusions - Synthesis, implications, recommendations
    6. References - Complete bibliography with clickable links
    
    Visual Design Requirements:
    - Professional CSS styling with responsive design
    - Interactive collapsible sections
    - Smooth scroll navigation
    - High contrast and semantic HTML for accessibility
    
    Citation Format:
    - Use numbered superscript citations [1]
    - Include author, title, source, date
    - Link to original sources where possible
    
    Special Sections:
    - Methodology box explaining research approach
    - Key insights highlights with confidence levels
    - Limitations section for transparency
    - Interactive elements for enhanced usability
  - Create HTML artifact with identifier: research-report-[topic]
  - Present final report compilation prompts

- **If report complete:**
  - Show completion summary
  - Offer options: view report, start new research, archive session

## Phase Transitions

### Planning → Gathering
**Trigger:** Research plan approved by user
**Actions:**
1. Ask Phase 2 clarifying questions:
   - Q1: Should I prioritize academic sources over industry reports?
   - Q2: Do you want me to focus on sources from specific regions or languages?
   - Q3: Should I avoid sources behind paywalls entirely?
   - Q4: What's the minimum publication date you'd consider relevant?
   - Allow user to skip or answer each question
2. Save answers to `[SESSION_ID]-phase2-answers.md`
3. Update metadata: `phase: 2, phase_name: "Information Gathering"`
4. Ensure `sources/` directory exists
5. Initialize `sources/source-inventory.md` with template:
   ```markdown
   # Source Inventory
   
   **Total Sources Collected**: 0
   **Date**: [Today's date]
   
   ## Sources by Sub-Question
   
   ### Sub-Question 1: [Question from plan]
   [To be populated]
   
   ### Sub-Question 2: [Question from plan]
   [To be populated]
   
   [Continue for all sub-questions]
   
   ## Source Quality Summary
   - Academic sources: 0
   - Industry reports: 0
   - News articles: 0
   - Technical documentation: 0
   - Other: 0
   
   ## Coverage Assessment
   - Well-covered topics: [To be identified]
   - Gaps identified: [To be identified]
   - Potential biases noted: [To be identified]
   ```
6. Set `current_action: "collect_sources"`
7. Load first sub-question from research plan

### Gathering → Analysis
**Trigger:** Sufficient sources collected (minimum 10-15 sources)
**Actions:**
1. Ask Phase 3 clarifying questions:
   - Q1: Should I focus on recent developments or include historical context?
   - Q2: Do you want detailed technical analysis or high-level insights?
   - Q3: Should I highlight areas of consensus or disagreement among sources?
   - Allow user to skip or answer each question
2. Save answers to `[SESSION_ID]-phase3-answers.md`
3. Update metadata: `phase: 3, phase_name: "Analysis"`
4. Set `current_action: "analyze_sources"`
5. Create `[SESSION_ID]-findings.md` with template:
   ```markdown
   # Analysis: [Research Topic]
   
   ## Executive Summary
   [2-3 paragraph overview of key findings and insights]
   
   ## Detailed Analysis by Research Question
   
   ### Question 1: [Original sub-question]
   
   #### Key Findings
   - **Finding 1**: [Description] (Source: [filename])
     - Supporting evidence: [quote or data]
     - Significance: [why this matters]
   
   #### Patterns and Insights
   - [Pattern 1]: [Description of pattern across sources]
   
   #### Contradictions or Gaps
   - [If any]: [Description] ([source1] vs [source2])
   
   [Repeat for each sub-question]
   
   ## Cross-Cutting Themes
   
   ### Theme 1: [Major theme across questions]
   [Synthesis paragraph]
   
   ## Conclusions and Insights
   
   ### Primary Conclusions
   1. [Major conclusion with evidence summary]
   
   ### Emerging Questions
   - [New questions raised by the research]
   
   ### Limitations
   - [Acknowledge any limitations in sources or analysis]
   
   ## Source Quality Assessment
   [Brief paragraph on overall source quality]
   
   ## Next Steps
   [Recommendations for report generation]
   ```
6. Load all collected sources for analysis

### Analysis → Report
**Trigger:** Findings document complete with all sub-questions addressed
**Actions:**
1. Ask Phase 4 clarifying questions:
   - Q1: Who is the primary audience for this report?
   - Q2: Should the report emphasize practical applications or theoretical insights?
   - Q3: Do you want interactive elements like collapsible sections or charts?
   - Allow user to skip or answer each question
2. Save answers to `[SESSION_ID]-phase4-answers.md`
3. Update metadata: `phase: 4, phase_name: "Report Generation"`
4. Set `current_action: "generate_report"`
5. Prepare report template with findings data
6. Load citation data for final report

### Report Complete
**Trigger:** Final HTML report generated and reviewed
**Actions:**
1. Update metadata: `phase: 5, phase_name: "Complete"`
2. Set `current_action: "session_complete"`
3. Archive session data
4. Clear `.deep-research/.current-research` file

## Error Handling

### Missing Files
- If session directory doesn't exist: Show error and suggest starting new research
- If critical files missing: Attempt to reconstruct from available data
- If metadata corrupted: Rebuild from directory contents and file timestamps

### Incomplete States
- If phase indicators don't match file existence: Reconcile based on actual files
- If progress seems inconsistent: Show warning and ask user to confirm current state
- If multiple sessions active: Show conflict resolution options

## Metadata Updates

Always update metadata.json when:
- Continuing from status check (update `last_updated`)
- Completing phase transitions (update `phase`, `phase_name`, `progress`)
- Completing major actions (add to `completed_actions`)
- Collecting new sources (update `total_sources`)
- Adding findings (update `findings_count`)

## Integration Points

- **TodoWrite Integration:** Sync with todo list for phase tracking
- **File System Integration:** Verify file existence matches metadata
- **Web Research Integration:** Continue from last search patterns
- **Citation Integration:** Maintain citation consistency across phases