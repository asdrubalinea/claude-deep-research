---
description: Display detailed information about the active research session and suggest next actions
allowed-tools:
  - Read
  - Write
  - Bash
  - TodoWrite
  - LS
---

# View Current Research Session

Display detailed information about the active research session.

## EXECUTION INSTRUCTIONS

### Step 1: Check for Active Session
```
CURRENT_SESSION_FILE = ".deep-research/.current-research"
if not file_exists(CURRENT_SESSION_FILE):
    display_error("No active research session found.")
    display_message("Use `/research-start [question]` to begin new research")
    display_message("Use `/research-list` to see all available sessions")
    exit
```

### Step 2: Load Session Data
```
ACTIVE_SESSION_ID = read_file(CURRENT_SESSION_FILE).strip()
if empty(ACTIVE_SESSION_ID):
    display_error("No active session ID found in .current-research file")
    exit

SESSION_META_DIR = ".deep-research/sessions/" + ACTIVE_SESSION_ID
METADATA_FILE = SESSION_META_DIR + "/metadata.json"

if not file_exists(METADATA_FILE):
    display_error("Session metadata not found: " + METADATA_FILE)
    display_message("Session may be corrupted. Try `/research-list` to see all sessions")
    exit

METADATA = parse_json(read_file(METADATA_FILE))
```

### Step 3: Validate Session Files
```
// Check for essential files and collect file status in parallel
PLAN_FILE = ACTIVE_SESSION_ID + "-plan.md"
SOURCES_DIR = "sources/"
FINDINGS_FILE = ACTIVE_SESSION_ID + "-findings.md"
REPORT_FILE = ACTIVE_SESSION_ID + "-report.html"

// Execute parallel file checks
FILE_STATUS = check_files_parallel([
    {"plan": PLAN_FILE},
    {"sources_dir": SOURCES_DIR},
    {"findings": FINDINGS_FILE},
    {"report": REPORT_FILE}
])
```

### Step 4: Collect Additional Session Data
```
// Count actual source files
SOURCE_COUNT = 0
SOURCE_FILES = []
if FILE_STATUS.sources_dir:
    SOURCE_FILES = list_files(SOURCES_DIR, "*.md")
    SOURCE_COUNT = count(SOURCE_FILES)

// Calculate time metrics
CURRENT_TIME = current_timestamp()
SESSION_DURATION = time_diff(METADATA.started, CURRENT_TIME)
LAST_ACTIVITY = time_diff(METADATA.lastUpdated, CURRENT_TIME)

// Determine current phase status
PHASE_STATUS = determine_phase_status(METADATA.phase, METADATA.progress)
```

### Step 5: Display Comprehensive Session Information
```
display_detailed_session_view(METADATA, FILE_STATUS, SOURCE_FILES, PHASE_STATUS)
```

## CLAUDE CODE IMPLEMENTATION

When executing this command, follow these exact steps:

### 1. Check for Active Session
```
Use Read tool to check: .deep-research/.current-research
If file doesn't exist or is empty:
  - Display: "❌ No active research session found."
  - Display: "💡 Use `/research-start [question]` to begin new research"
  - Display: "📋 Use `/research-list` to see all available sessions"
  - Exit
```

### 2. Load Session Metadata
```
Parse session ID from .current-research file
Construct session metadata path: .deep-research/sessions/[SESSION_ID]
Use Read tool to load metadata.json
If metadata.json doesn't exist, display error and exit
Parse JSON metadata into usable structure
```

### 3. Analyze Session Files
```
Check existence of all expected files:
- [SESSION_ID]-plan.md
- sources/ directory
- [SESSION_ID]-findings.md (if analysis phase reached)
- [SESSION_ID]-report.html (if report phase reached)
Use LS tool to count source files in sources/ directory
```

### 4. Calculate Session Metrics
```
Calculate time elapsed since session start
Calculate time since last update
Determine current phase and progress percentage
Count sub-questions by status (pending, in_progress, completed)
Analyze source distribution by type and tier
```

### 5. Display Comprehensive Session View
```
Use exact format specified in "Display Format" section below
Include all calculated metrics and file status
Show detailed progress breakdown
List recent activity and next steps
```

## Description

This command provides a comprehensive view of your current research session, including all phases, collected sources, findings, and detailed progress metrics. It's designed for in-depth session analysis and planning.

## Display Format

### Header Section
```
🔬 ACTIVE RESEARCH SESSION: [Session Title]
════════════════════════════════════════════════════════════════════════════════

📋 Session ID: [session-id]
❓ Research Question: [original research question]
🕐 Started: [date and time] ([X] hours/days ago)
🔄 Last Updated: [date and time] ([X] minutes/hours ago)
📊 Status: [Active/Paused] | Phase: [X/4] [Phase Name]
```

### Progress Overview
```
🎯 RESEARCH PROGRESS
────────────────────────────────────────────────────────────────────────────────

Phase 1 - Planning:     [✅ Complete | ⏳ In Progress | ⏸️ Pending]
Phase 2 - Gathering:    [✅ Complete | ⏳ In Progress | ⏸️ Pending]
Phase 3 - Analysis:     [✅ Complete | ⏳ In Progress | ⏸️ Pending]
Phase 4 - Report:       [✅ Complete | ⏳ In Progress | ⏸️ Pending]

Overall Progress: [████████░░] [X]% Complete
```

### Session Statistics
```
📊 SESSION METRICS
────────────────────────────────────────────────────────────────────────────────

Research Activity:
• Sub-questions defined: [X] total
• Sources collected: [X] of [target] planned
• Findings documented: [Yes/No]
• Citations generated: [X] total

Time Investment:
• Total time spent: [X]h [Y]m
• Session count: [X] sessions
• Words generated: [X,XXX]
• Last activity: [X] minutes/hours ago

File Status:
• Research plan: [✅ Exists | ❌ Missing] ([file size])
• Sources directory: [✅ Exists | ❌ Missing] ([X] files)
• Findings document: [✅ Exists | ❌ Missing] ([file size])
• Final report: [✅ Exists | ❌ Missing] ([file size])
```

### Sub-Questions Progress
```
🔍 SUB-QUESTION PROGRESS
────────────────────────────────────────────────────────────────────────────────

1. [Sub-question text] [✅ Researched | ⏳ In Progress | ⏸️ Pending]
   Sources: [X] collected | Priority: [High/Medium/Low]

2. [Sub-question text] [✅ Researched | ⏳ In Progress | ⏸️ Pending]
   Sources: [X] collected | Priority: [High/Medium/Low]

[... continue for all sub-questions ...]

Progress Summary:
• Completed: [X] sub-questions
• In Progress: [X] sub-questions  
• Pending: [X] sub-questions
```

### Source Analysis
```
📚 SOURCE COLLECTION ANALYSIS
────────────────────────────────────────────────────────────────────────────────

Collection Overview:
• Total sources: [X] collected
• Target sources: [X] planned
• Collection rate: [X]% complete

By Source Type:
• Academic papers: [X] sources
• Industry reports: [X] sources
• News articles: [X] sources
• Blog posts: [X] sources
• Surveys/Studies: [X] sources
• Other: [X] sources

By Quality Tier:
• Tier 1 (Peer-reviewed): [X] sources ([X]%)
• Tier 2 (Professional): [X] sources ([X]%)
• Tier 3 (General): [X] sources ([X]%)

Well-Covered Topics:
• [Topic 1]
• [Topic 2]
• [Topic 3]

Research Gaps Identified:
• [Gap 1]
• [Gap 2]
• [Gap 3]

Potential Biases Noted:
• [Bias 1]
• [Bias 2]
• [Bias 3]
```

### Current Phase Details
```
🎯 CURRENT PHASE: [Phase Name]
────────────────────────────────────────────────────────────────────────────────

Phase Status: [Detailed phase-specific status]
Current Focus: [What you're working on now]

[Phase-specific information based on current phase]
```

### Phase-Specific Details

#### Phase 1 - Planning
```
Planning Status: [In Progress/Complete/Needs Review]
Sub-questions: [X] defined
Plan approved: [Yes/No]
Next action: [Review plan/Get approval/Proceed to gathering]
```

#### Phase 2 - Information Gathering
```
Gathering Status: [In Progress/Complete]
Sources collected: [X] of [target] planned
Current sub-question: [X]. [Question text]
Search strategy: [Current search approach]
Next action: [Collect sources for SQ[X]/Review source quality/Proceed to analysis]
```

#### Phase 3 - Analysis
```
Analysis Status: [In Progress/Complete]
Sources analyzed: [X] of [total] collected
Findings documented: [Yes/No]
Current focus: [What aspects being analyzed]
Next action: [Continue analysis/Generate findings/Proceed to report]
```

#### Phase 4 - Report Generation
```
Report Status: [In Progress/Complete]
Report format: [HTML/PDF/Markdown]
Sections completed: [X] of [total]
Citations formatted: [X] total
Next action: [Continue report/Review report/Finalize session]
```

### Next Steps
```
⏭️ IMMEDIATE NEXT STEPS
────────────────────────────────────────────────────────────────────────────────

1. [Primary next action with specific details]
2. [Secondary next action]
3. [Tertiary next action]

Recommendations:
• [Strategic recommendation 1]
• [Strategic recommendation 2]
• [Strategic recommendation 3]

Available Commands:
• `/research-status` - Continue research workflow
• `/research-list` - View all sessions
• `/research-end` - Complete and finalize session
```

### Footer
```
════════════════════════════════════════════════════════════════════════════════
💡 Use `/research-status` to continue your research workflow
📋 Use `/research-list` to switch to a different session
🔚 Use `/research-end` to complete and finalize this session
════════════════════════════════════════════════════════════════════════════════
```

## Display Components

### 1. Session Header
- Session ID and title
- Research question
- Timestamps and duration
- Overall status and phase

### 2. Progress Overview
- Visual progress bar for overall completion
- Phase-by-phase status indicators
- Completion percentages

### 3. Session Statistics
- Research activity metrics
- Time investment tracking
- File status verification
- Output quantification

### 4. Sub-Question Analysis
- Individual sub-question progress
- Source collection per question
- Priority and status tracking
- Completion summary

### 5. Source Collection Analysis
- Total source counts and targets
- Source type distribution
- Quality tier analysis
- Coverage and gap identification

### 6. Current Phase Details
- Phase-specific status information
- Current focus and activities
- Next actions and recommendations

### 7. Navigation and Actions
- Available commands
- Workflow continuation options
- Session management actions

## Session Details

### Research Progress Calculation
```
OVERALL_PROGRESS = (
    (PLANNING_WEIGHT * planning_completion) +
    (GATHERING_WEIGHT * gathering_completion) +
    (ANALYSIS_WEIGHT * analysis_completion) +
    (REPORT_WEIGHT * report_completion)
) / 100

Where:
- PLANNING_WEIGHT = 15%
- GATHERING_WEIGHT = 40%
- ANALYSIS_WEIGHT = 30%
- REPORT_WEIGHT = 15%
```

### Sub-Question Status Logic
```
For each sub-question:
- "Pending": status = "pending", sources = 0
- "In Progress": status = "in_progress", sources > 0
- "Researched": status = "researched", sufficient sources collected
- "Complete": status = "complete", included in findings
```

### Source Quality Assessment
```
Tier 1 Sources (Highest Quality):
- Peer-reviewed academic papers
- Government reports and official statistics
- Established research institutions
- Regulatory bodies and official agencies

Tier 2 Sources (Professional Quality):
- Industry reports from recognized firms
- Professional publications and journals
- Established news organizations
- Expert interviews and statements

Tier 3 Sources (General Quality):
- Blog posts and opinion pieces
- Social media content
- Unverified surveys or studies
- Commercial marketing materials
```

## Progress Metrics

### Time Tracking
- **Session Duration**: Total time from start to current moment
- **Active Time**: Time spent in active research (tracked via updates)
- **Last Activity**: Time since last metadata update
- **Average Session Length**: Total time / number of sessions

### Research Output Metrics
- **Words Generated**: Total word count across all documents
- **Citations Created**: Count of properly formatted citations
- **Sources Collected**: Total number of source documents
- **Findings Documented**: Boolean indicator of analysis completion

### Quality Metrics
- **Source Quality Distribution**: Percentage by tier level
- **Research Coverage**: Topics with sufficient sources vs. gaps
- **Bias Identification**: Number of potential biases noted
- **Cross-Referencing**: Sources that validate each other

### Progress Indicators
- **Phase Completion**: Percentage complete for each phase
- **Sub-Question Progress**: Individual question research status
- **Target Achievement**: Sources collected vs. planned targets
- **Quality Standards**: Adherence to source quality requirements

## Source Analysis

### Source Collection Overview
Display comprehensive view of all collected sources with:
- Total count and target achievement
- Source type distribution
- Quality tier analysis
- Collection timeline
- Coverage assessment

### Source Quality Assessment
Evaluate and display:
- **Tier 1 Sources**: Count, percentage, specific examples
- **Tier 2 Sources**: Count, percentage, specific examples  
- **Tier 3 Sources**: Count, percentage, specific examples
- **Quality Trends**: Improvement or decline over time

### Coverage Analysis
Identify and display:
- **Well-Covered Topics**: Areas with sufficient, high-quality sources
- **Research Gaps**: Under-researched areas requiring attention
- **Bias Identification**: Potential biases in source selection
- **Validation Opportunities**: Sources that can cross-reference each other

### Source File Analysis
For each source file in sources/:
- File name and title
- Source type and tier classification
- Sub-question relevance
- Quality assessment
- Key findings or quotes

## Next Steps

### Phase-Specific Recommendations

#### Planning Phase
- Review and refine research plan
- Adjust sub-questions based on feasibility
- Get user approval for plan
- Prepare for source collection

#### Gathering Phase
- Continue collecting sources for priority sub-questions
- Maintain source quality standards
- Fill identified research gaps
- Validate source credibility

#### Analysis Phase
- Synthesize findings from collected sources
- Document key insights and themes
- Identify conflicting information
- Prepare analysis summary

#### Report Phase
- Generate comprehensive final report
- Ensure all citations are properly formatted
- Review for completeness and accuracy
- Prepare session finalization

### Strategic Recommendations
Based on current session state:
- **Quality Improvements**: Suggestions for better source quality
- **Efficiency Gains**: Ways to optimize research process
- **Gap Filling**: Priority areas needing more research
- **Validation Needs**: Claims requiring additional verification

### Available Actions
- **Continue Research**: Use `/research-status` to proceed
- **Switch Sessions**: Use `/research-list` to see other sessions
- **End Session**: Use `/research-end` to finalize research
- **Export Data**: Options for sharing or archiving research

## What would you like to do next?
1. Continue with current research session (`/research-status`)
2. View all research sessions (`/research-list`)
3. Start a new research session (`/research-start [question]`)
4. Finalize current research (`/research-end`)
5. Just show me the current status without action

## Error Handling

### Missing Files
- **Session Directory**: Provide recovery suggestions
- **Metadata File**: Attempt reconstruction from available data
- **Source Files**: Note missing sources and suggest re-collection
- **Plan File**: Offer to regenerate from metadata

### Corrupted Data
- **Invalid JSON**: Attempt repair or provide manual fix instructions
- **Inconsistent State**: Reconcile metadata with actual file system
- **Broken References**: Update file paths and references
- **Missing Timestamps**: Estimate from file system metadata

### Display Fallbacks
- **Incomplete Metrics**: Show available data with gaps noted
- **Missing Descriptions**: Use fallback text with clear indicators
- **Calculation Errors**: Show raw data when calculations fail
- **Formatting Issues**: Ensure readable output even with display problems

## Important Rules

### Data Integrity
1. **Verify Before Display**: Check all file existence before showing status
2. **Validate Metrics**: Ensure calculations are accurate and meaningful
3. **Consistent Formatting**: Use exact format specifications
4. **Error Transparency**: Clearly indicate any data issues or limitations

### User Experience
1. **Comprehensive View**: Show all relevant information without overwhelming
2. **Actionable Information**: Include specific next steps and recommendations
3. **Navigation Support**: Provide clear options for continuing work
4. **Progress Visibility**: Make research progress immediately apparent

### Session Management
1. **State Verification**: Confirm session state matches file system
2. **Metadata Synchronization**: Ensure metadata reflects actual progress
3. **File System Consistency**: Check for orphaned or missing files
4. **Recovery Options**: Provide ways to fix common issues

### Quality Standards
1. **Source Quality Display**: Clearly show source tier distribution
2. **Research Coverage**: Highlight gaps and well-covered areas
3. **Bias Awareness**: Surface potential biases in source selection
4. **Citation Tracking**: Show progress toward complete citation records