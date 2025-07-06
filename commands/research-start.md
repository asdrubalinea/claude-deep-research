# Start Research Session

Begin deep research for: $ARGUMENTS

## EXECUTION INSTRUCTIONS

### Step 1: Parse and Validate Input
```
RESEARCH_QUESTION = "$ARGUMENTS"
if empty(RESEARCH_QUESTION):
    error("Please provide a research question. Usage: /research-start [your research question]")
    exit
```

### Step 2: Generate Session Identifier
```
TIMESTAMP = current_datetime_format("YYYY-MM-DD-HHMM")
TOPIC_SLUG = generate_slug(RESEARCH_QUESTION)
SESSION_ID = TIMESTAMP + "-" + TOPIC_SLUG
SESSION_DIR = "/home/irene/deep-research/sessions/" + SESSION_ID
```

### Step 3: Check for Existing Active Session
```
if file_exists("/home/irene/deep-research/sessions/.current-research"):
    ACTIVE_SESSION = read_file("/home/irene/deep-research/sessions/.current-research")
    if not empty(ACTIVE_SESSION):
        warn("Active session detected: " + ACTIVE_SESSION)
        prompt("Would you like to continue with the existing session or start a new one?")
        // Handle user choice
```

### Step 4: Initialize Session Directory
```
create_directory(SESSION_DIR)
if not directory_exists(SESSION_DIR):
    error("Failed to create session directory: " + SESSION_DIR)
    exit
```

### Step 5: Create Initial Metadata
```
METADATA = {
    "id": SESSION_ID,
    "title": derive_title(RESEARCH_QUESTION),
    "question": RESEARCH_QUESTION,
    "started": current_iso_timestamp(),
    "lastUpdated": current_iso_timestamp(),
    "status": "active",
    "phase": "planning",
    // ... (complete metadata structure as defined above)
}
write_json(SESSION_DIR + "/metadata.json", METADATA)
```

### Step 6: Execute Phase 1 Research Planning
```
// Apply the complete Phase 1 planning methodology
RESEARCH_PLAN = execute_phase_1_planning(RESEARCH_QUESTION)
write_file(SESSION_DIR + "/01-plan.md", RESEARCH_PLAN)
```

### Step 7: Update Global State
```
write_file("/home/irene/deep-research/sessions/.current-research", SESSION_ID)
```

### Step 8: Initialize Progress Tracking
```
create_todo_list([
    {id: "phase1", content: "Research Planning", status: "in_progress", priority: "high"},
    {id: "phase2", content: "Information Gathering", status: "pending", priority: "high"},
    {id: "phase3", content: "Analysis & Synthesis", status: "pending", priority: "medium"},
    {id: "phase4", content: "Report Generation", status: "pending", priority: "medium"}
])
```

### Step 9: Provide User Feedback and Request Approval
```
display_session_summary(SESSION_ID, RESEARCH_PLAN)
prompt_user_approval("Ready to proceed with information gathering?")
```

## CLAUDE CODE IMPLEMENTATION

When executing this command, follow these exact steps:

### 1. Parse Research Question
```
Extract the research question from $ARGUMENTS
If empty, respond with: "Please provide a research question. Usage: /research-start [your research question]"
```

### 2. Generate Session ID and Paths
```
Get current timestamp in YYYY-MM-DD-HHMM format
Create topic slug from research question (3-5 key words, lowercase, hyphenated)
Combine as: TIMESTAMP-TOPIC_SLUG
Set SESSION_DIR as: /home/irene/deep-research/sessions/[SESSION_ID]
```

### 3. Check for Active Session
```
Use Read tool to check: /home/irene/deep-research/sessions/.current-research
If file exists and not empty, warn user about existing active session
Ask: "Continue existing session or start new one?"
```

### 4. Create Session Directory
```
Use Bash tool: mkdir -p "/home/irene/deep-research/sessions/[SESSION_ID]"
Verify directory creation was successful
```

### 5. Initialize Metadata
```
Create metadata.json with the complete structure defined above
Use Write tool to save to: [SESSION_DIR]/metadata.json
Include actual timestamp, session ID, and research question
```

### 6. Execute Phase 1 Planning
```
Transform into expert research strategist role
Apply the complete Phase 1 planning methodology from prompts/PHASE-1-PLANNING.md
Analyze research question and create 5-7 sub-questions with:
- Specific, answerable questions
- Search strategies with keywords
- Source types to prioritize
- Priority levels (High/Medium/Low)
- Potential challenges
Create comprehensive research plan following the template
```

### 7. Save Research Plan
```
Use Write tool to save plan as: [SESSION_DIR]/01-plan.md
Follow exact template format from prompts/PHASE-1-PLANNING.md
Include all required sections: Overview, Sub-Questions, Research Sequence, Expected Outcomes, Synthesis Approach
```

### 8. Update Global State
```
Use Write tool to save session ID to: /home/irene/deep-research/sessions/.current-research
This marks the session as active
```

### 9. Initialize TodoWrite
```
Use TodoWrite tool to create four-phase tracking:
- Phase 1: Research Planning (status: in_progress)
- Phase 2: Information Gathering (status: pending)
- Phase 3: Analysis & Synthesis (status: pending)
- Phase 4: Report Generation (status: pending)
```

### 10. Provide Summary and Request Approval
```
Confirm successful session creation with session ID
Provide brief summary of research plan with sub-question count
Show file locations created
Ask: "I've created a comprehensive research plan with [X] sub-questions. Would you like to review and approve this plan before I proceed with information gathering? You can also suggest modifications if needed."
```

### 11. Mark Planning Complete (After Approval)
```
Update metadata.json progress.planning.status to "completed"
Update metadata.json progress.planning.planApproved to true
Update TodoWrite to mark Phase 1 as completed
Transition to Phase 2 ready state
```

## Description

This command initiates a new research session using the four-phase deep research methodology. It creates a structured workspace, establishes session metadata, and guides you through the research planning phase.

## Workflow Instructions

### Command Execution Flow
1. **Parse Input**: Extract research question from arguments
2. **Generate Session ID**: Create timestamp-based session identifier
3. **Initialize Directory**: Create structured session workspace
4. **Create Metadata**: Initialize session tracking with JSON metadata
5. **Execute Planning Phase**: Apply Phase 1 research planning methodology
6. **Save Research Plan**: Store comprehensive research plan as 01-plan.md
7. **Update Global State**: Set as active session in .current-research
8. **Track Progress**: Initialize TodoWrite with four-phase workflow
9. **Request Approval**: Ask user to review and approve plan before proceeding

### Session Directory Structure
```
sessions/YYYY-MM-DD-HHMM-slug/
├── metadata.json           # Session state and progress tracking
├── 01-plan.md             # Research plan with sub-questions
├── 02-sources/            # Source collection directory (created later)
├── 03-findings.md         # Analysis results (created later)
└── 04-report.html         # Final report (created later)
```

### Topic Slug Generation
- Extract key terms from research question
- Convert to lowercase, hyphenated format
- Limit to 3-5 words maximum
- Remove stop words (the, and, of, etc.)
- Examples:
  - "How do AI tools impact productivity?" → "ai-tools-productivity"
  - "Climate change effects on agriculture" → "climate-change-agriculture"
  - "Remote work trends in 2024" → "remote-work-trends-2024"

## Phase 1: Research Planning

### Planning Objectives
Transform the user's research question into a comprehensive, actionable research strategy without conducting actual research. Focus on strategic decomposition and systematic planning.

### Step 1: Research Question Analysis
- **Identify Core Topic**: Extract the main subject and scope
- **Determine Perspectives**: Consider multiple angles and viewpoints
- **Assess Constraints**: Note any limitations or specific requirements
- **Evaluate Complexity**: Gauge the depth and breadth needed

### Step 2: Sub-Question Generation (5-7 questions)
Create specific, answerable sub-questions that collectively address the main research question. For each sub-question, specify:

**Required Elements:**
- **Question**: Clear, specific, and directly answerable
- **Search Strategy**: Keywords, phrases, and boolean operators
- **Source Types**: Academic papers, industry reports, news articles, government data, etc.
- **Priority**: High/Medium/Low based on importance to main question
- **Challenges**: Anticipated difficulties (paywalls, recency, bias, availability)

**Quality Criteria:**
- Questions should be mutually exclusive but collectively exhaustive
- Each question should lead to concrete, verifiable information
- Questions should build upon each other logically
- Mix of quantitative and qualitative research needs

### Step 3: Research Sequence Planning
- **Primary Questions**: Start with foundational questions that inform others
- **Secondary Questions**: Build on primary findings
- **Synthesis Questions**: Connect findings to answer main question
- **Validation Questions**: Cross-check and verify key claims

### Step 4: Expected Outcomes Definition
For each sub-question, define:
- **Information Type**: Statistics, expert opinions, case studies, etc.
- **Quality Standards**: Source credibility requirements
- **Synthesis Role**: How findings will contribute to final answer
- **Success Metrics**: What constitutes sufficient research for each question

### Step 5: Research Plan Documentation
Create comprehensive plan following the template structure with:
- Clear overview explaining research scope and objectives
- Detailed sub-questions with full specifications
- Logical research sequence with rationale
- Expected outcomes and synthesis approach
- Notes on limitations and special considerations

## Session Management

### Session Initialization
1. **Generate Session ID**: Format `YYYY-MM-DD-HHMM-slug`
   - Use current timestamp for uniqueness
   - Create descriptive slug from research question
   - Ensure no conflicts with existing sessions

2. **Create Session Directory**: 
   - Path: `/home/irene/deep-research/sessions/[SESSION_ID]/`
   - Verify parent directory exists
   - Create with proper permissions

3. **Initialize Metadata**: Create metadata.json with:
   - Session identifiers and timestamps
   - Research question and derived title
   - Initial status and phase information
   - Empty progress tracking structure
   - File path references

### Global State Management
1. **Update .current-research**: 
   - Store active session ID
   - Include last update timestamp
   - Ensure atomic write operations

2. **State Validation**:
   - Check for existing active sessions
   - Warn user if abandoning previous session
   - Offer to complete or pause existing work

### Progress Tracking Integration
1. **TodoWrite Initialization**:
   - Create four-phase task list
   - Mark planning phase as in_progress
   - Set other phases as pending
   - Include session context in task descriptions

2. **Metadata Updates**:
   - Record planning phase completion
   - Track sub-question count
   - Note plan approval status
   - Update last modified timestamps

### Error Handling
- **Directory Creation Failures**: Provide clear error messages and alternatives
- **Metadata Corruption**: Validate JSON structure and provide recovery
- **State Conflicts**: Handle concurrent session attempts gracefully
- **Permission Issues**: Guide user through access problems

## Metadata Structure

### Initial metadata.json Template
```json
{
  "id": "YYYY-MM-DD-HHMM-slug",
  "title": "Human-readable research title",
  "question": "Original research question from user",
  "started": "ISO-8601-timestamp",
  "lastUpdated": "ISO-8601-timestamp",
  "status": "active",
  "phase": "planning",
  "progress": {
    "planning": {
      "status": "in_progress",
      "subQuestions": 0,
      "planApproved": false
    },
    "gathering": {
      "status": "pending",
      "sourcesCollected": 0,
      "sourcesTargeted": 0
    },
    "analysis": {
      "status": "pending",
      "findingsGenerated": false
    },
    "report": {
      "status": "pending",
      "format": "html"
    }
  },
  "research": {
    "subQuestions": [],
    "sources": {
      "total": 0,
      "byType": {},
      "byTier": {}
    },
    "coverage": {
      "wellCovered": [],
      "gaps": [],
      "biasesNoted": []
    }
  },
  "files": {
    "planFile": "01-plan.md",
    "sourcesDir": "02-sources/",
    "findingsFile": "03-findings.md",
    "reportFile": "04-report.html"
  },
  "metrics": {
    "totalTimeSpent": "0h 0m",
    "sessionsCount": 1,
    "wordsGenerated": 0,
    "citationsCount": 0
  }
}
```

### Metadata Field Descriptions
- **id**: Unique session identifier (timestamp + slug)
- **title**: Human-readable research title derived from question
- **question**: Original research question exactly as provided
- **started/lastUpdated**: ISO-8601 timestamps for session tracking
- **status**: active|completed|paused|abandoned
- **phase**: planning|gathering|analysis|report
- **progress**: Phase-specific progress tracking
- **research**: Research content and quality metrics
- **files**: File path references within session
- **metrics**: Time tracking and output measurements

### Metadata Update Points
- **Session Creation**: Initialize with default values
- **Plan Completion**: Update sub-question count and approval status
- **Phase Transitions**: Change phase and update progress status
- **Source Collection**: Increment source counts and update types
- **Session Completion**: Mark as completed and finalize metrics

## Important Rules

### Research Planning Phase Rules
1. **NO ACTUAL RESEARCH**: Do not use WebSearch, WebFetch, or conduct any research
2. **PLANNING ONLY**: Focus exclusively on strategic planning and decomposition
3. **COMPREHENSIVE PLANNING**: Create detailed sub-questions with full specifications
4. **SAVE BEFORE APPROVAL**: Always save the research plan before requesting user approval
5. **QUALITY STANDARDS**: Ensure sub-questions are specific, answerable, and mutually exclusive

### Session Management Rules
1. **UNIQUE SESSIONS**: Each session must have a unique timestamp-based ID
2. **ATOMIC OPERATIONS**: Complete all file operations or roll back on failure
3. **STATE CONSISTENCY**: Keep metadata.json and .current-research synchronized
4. **DIRECTORY STRUCTURE**: Follow exact directory naming and file organization
5. **METADATA INTEGRITY**: Validate JSON structure and required fields

### User Interaction Rules
1. **CLEAR COMMUNICATION**: Provide detailed progress updates and next steps
2. **APPROVAL REQUIRED**: Always request user approval before proceeding to Phase 2
3. **MODIFICATION SUPPORT**: Allow users to request changes to research plan
4. **ERROR TRANSPARENCY**: Clearly explain any issues or failures
5. **PROGRESS VISIBILITY**: Show comprehensive session status and plan summary

### File Management Rules
1. **ABSOLUTE PATHS**: Use absolute paths for all file operations
2. **CONSISTENT NAMING**: Follow exact naming conventions for all files
3. **BACKUP SAFETY**: Never overwrite existing sessions without confirmation
4. **PROPER PERMISSIONS**: Ensure all created files have appropriate access
5. **CLEAN STRUCTURE**: Maintain organized directory hierarchy

### Execution Sequence (Critical)
1. **Parse research question** from command arguments
2. **Generate session ID** using timestamp and topic slug
3. **Create session directory** with proper structure
4. **Initialize metadata.json** with complete structure
5. **Execute Phase 1 planning** using proven methodology
6. **Save research plan** as 01-plan.md
7. **Update .current-research** with new session
8. **Initialize TodoWrite** with four-phase tasks
9. **Provide summary** and request user approval
10. **Mark planning phase complete** only after user approval

### Validation Requirements
- **Question Completeness**: Must have 5-7 well-formed sub-questions
- **Search Strategy**: Each sub-question needs specific search approach
- **Source Types**: Identify appropriate source types for each question
- **Priority Assignment**: Assign meaningful priority levels
- **Challenge Assessment**: Identify potential research obstacles
- **Synthesis Plan**: Explain how findings will address main question

### Success Indicators
- Research plan saved successfully to 01-plan.md
- Metadata.json created with valid structure
- Global state updated in .current-research
- TodoWrite initialized with four phases
- User presented with clear approval request
- Planning phase marked complete after approval

## RESEARCH PLAN TEMPLATE

Use this exact template when creating the 01-plan.md file:

```markdown
# Research Plan: [DERIVED_TITLE]

**Date**: [CURRENT_DATE]
**Session ID**: [SESSION_ID]
**Research Question**: [ORIGINAL_QUESTION]

## Overview
[2-3 sentences explaining the research scope, why this question matters, and what the research aims to achieve]

## Sub-Questions

### 1. [First Sub-Question]
- **Question**: [Specific, answerable question]
- **Search Strategy**: [Keywords, phrases, boolean operators]
- **Source Types**: [Academic papers, industry reports, news articles, etc.]
- **Priority**: [High/Medium/Low]
- **Potential Challenges**: [Paywalls, recency, availability, bias, etc.]

### 2. [Second Sub-Question]
- **Question**: [Specific, answerable question]
- **Search Strategy**: [Keywords, phrases, boolean operators]
- **Source Types**: [Academic papers, industry reports, news articles, etc.]
- **Priority**: [High/Medium/Low]
- **Potential Challenges**: [Paywalls, recency, availability, bias, etc.]

### 3. [Third Sub-Question]
- **Question**: [Specific, answerable question]
- **Search Strategy**: [Keywords, phrases, boolean operators]
- **Source Types**: [Academic papers, industry reports, news articles, etc.]
- **Priority**: [High/Medium/Low]
- **Potential Challenges**: [Paywalls, recency, availability, bias, etc.]

### 4. [Fourth Sub-Question]
- **Question**: [Specific, answerable question]
- **Search Strategy**: [Keywords, phrases, boolean operators]
- **Source Types**: [Academic papers, industry reports, news articles, etc.]
- **Priority**: [High/Medium/Low]
- **Potential Challenges**: [Paywalls, recency, availability, bias, etc.]

### 5. [Fifth Sub-Question]
- **Question**: [Specific, answerable question]
- **Search Strategy**: [Keywords, phrases, boolean operators]
- **Source Types**: [Academic papers, industry reports, news articles, etc.]
- **Priority**: [High/Medium/Low]
- **Potential Challenges**: [Paywalls, recency, availability, bias, etc.]

### 6. [Sixth Sub-Question] (Optional)
- **Question**: [Specific, answerable question]
- **Search Strategy**: [Keywords, phrases, boolean operators]
- **Source Types**: [Academic papers, industry reports, news articles, etc.]
- **Priority**: [High/Medium/Low]
- **Potential Challenges**: [Paywalls, recency, availability, bias, etc.]

### 7. [Seventh Sub-Question] (Optional)
- **Question**: [Specific, answerable question]
- **Search Strategy**: [Keywords, phrases, boolean operators]
- **Source Types**: [Academic papers, industry reports, news articles, etc.]
- **Priority**: [High/Medium/Low]
- **Potential Challenges**: [Paywalls, recency, availability, bias, etc.]

## Research Sequence
1. **Start with**: [Which sub-question to tackle first and why]
2. **Then move to**: [Second priority question and rationale]
3. **Follow with**: [Third priority question and rationale]
4. **Continue with**: [Remaining questions in logical order]
5. **Conclude with**: [Final questions that synthesize or validate]

## Expected Outcomes
- **From Q1**: [What specific insights, data, or information we expect to find]
- **From Q2**: [What specific insights, data, or information we expect to find]
- **From Q3**: [What specific insights, data, or information we expect to find]
- **From Q4**: [What specific insights, data, or information we expect to find]
- **From Q5**: [What specific insights, data, or information we expect to find]
- **From Q6**: [What specific insights, data, or information we expect to find] (if applicable)
- **From Q7**: [What specific insights, data, or information we expect to find] (if applicable)

## Synthesis Approach
[Detailed explanation of how findings from each sub-question will be combined to answer the main research question. Include:
- How different types of evidence will be weighted
- What patterns or themes to look for across sources
- How to reconcile conflicting information
- What gaps might remain and how to address them]

## Quality Standards
- **Tier 1 Sources**: Academic papers, government reports, peer-reviewed studies
- **Tier 2 Sources**: Industry reports, established news organizations, professional publications
- **Tier 3 Sources**: Blog posts, opinion pieces, social media (use with caution)
- **Citation Requirements**: All claims must be properly cited with accessible sources
- **Bias Awareness**: Note potential biases in sources and seek multiple perspectives

## Notes
[Any additional considerations, limitations, constraints, or special instructions for the research process]

## Next Steps
After approval of this plan:
1. Begin Phase 2: Information Gathering
2. Create 02-sources/ directory for source collection
3. Start with highest priority sub-questions
4. Maintain source inventory and quality tracking
5. Proceed through systematic source collection process
```

## SLUG GENERATION EXAMPLES

Topic slug creation from research questions:

**Input**: "How do AI productivity tools impact knowledge worker performance?"
**Slug**: "ai-productivity-knowledge-workers"

**Input**: "What are the long-term effects of remote work on employee satisfaction?"
**Slug**: "remote-work-employee-satisfaction"

**Input**: "Climate change adaptation strategies for coastal cities in 2024"
**Slug**: "climate-adaptation-coastal-cities"

**Input**: "Effectiveness of mindfulness meditation for reducing workplace stress"
**Slug**: "mindfulness-workplace-stress"

**Input**: "How do electric vehicle adoption trends vary by demographics?"
**Slug**: "ev-adoption-demographics"

## TITLE GENERATION EXAMPLES

Research question to title conversion:

**Question**: "How do AI productivity tools impact knowledge worker performance?"
**Title**: "AI Productivity Tools: Impact on Knowledge Worker Performance"

**Question**: "What are the long-term effects of remote work on employee satisfaction?"
**Title**: "Remote Work: Long-term Effects on Employee Satisfaction"

**Question**: "Climate change adaptation strategies for coastal cities in 2024"
**Title**: "Climate Change Adaptation Strategies for Coastal Cities"

**Question**: "Effectiveness of mindfulness meditation for reducing workplace stress"
**Title**: "Mindfulness Meditation: Effectiveness for Workplace Stress Reduction"