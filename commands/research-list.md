---
description: Enhanced session directory with analytics, smart filtering, and optimization recommendations
allowed-tools:
  - Read
  - Write
  - Bash
  - LS
  - mcp__memory__create_memory
  - mcp__memory__search_memory
  - mcp__memory__update_memory
  - mcp__sequentialthinking__start_thinking
  - mcp__sequentialthinking__continue_thinking
  - mcp__sequentialthinking__finish_thinking
---

# Enhanced Session Directory

Display intelligent overview of all research sessions with smart analytics.

## 🚀 Enhanced Directory Features
- **Smart Analytics**: Session performance metrics and quality scoring with historical comparison
- **Memory-Powered Insights**: Pattern recognition from persistent research history
- **Intelligent Filtering**: Auto-categorization with optimization suggestions
- **Sequential Analysis**: Deep thinking for workspace optimization and session prioritization
- **Quick Actions**: Smart recommendations for session management based on learned patterns

## EXECUTION INSTRUCTIONS

### Step 1: Discover All Sessions
```
SESSIONS_DIR = ".deep-research/sessions"
SESSION_DIRECTORIES = list_directories(SESSIONS_DIR)
SESSIONS = []

// Process all session metadata files in parallel
METADATA_FILES = []
for each SESSION_DIR in SESSION_DIRECTORIES:
    if SESSION_DIR != ".current-research":
        METADATA_FILE = SESSIONS_DIR + "/" + SESSION_DIR + "/metadata.json"
        if file_exists(METADATA_FILE):
            METADATA_FILES.append(METADATA_FILE)

// Read all metadata files in parallel
SESSIONS = read_json_files_parallel(METADATA_FILES)
```

### Step 2: Identify Active Session
```
CURRENT_SESSION_FILE = ".deep-research/.current-research"
ACTIVE_SESSION_ID = null
if file_exists(CURRENT_SESSION_FILE):
    ACTIVE_SESSION_ID = read_file(CURRENT_SESSION_FILE).strip()
```

### Step 3: Categorize Sessions
```
ACTIVE_SESSIONS = []
COMPLETED_SESSIONS = []
PAUSED_SESSIONS = []
ABANDONED_SESSIONS = []

for each SESSION in SESSIONS:
    switch SESSION.status:
        case "active": ACTIVE_SESSIONS.append(SESSION)
        case "completed": COMPLETED_SESSIONS.append(SESSION)
        case "paused": PAUSED_SESSIONS.append(SESSION)
        case "abandoned": ABANDONED_SESSIONS.append(SESSION)
```

### Step 4: Sort Sessions
```
// Sort by last updated timestamp (most recent first)
ACTIVE_SESSIONS.sort(by: "lastUpdated", desc: true)
COMPLETED_SESSIONS.sort(by: "lastUpdated", desc: true)
PAUSED_SESSIONS.sort(by: "lastUpdated", desc: true)
ABANDONED_SESSIONS.sort(by: "lastUpdated", desc: true)
```

### Step 5: Display Session List
```
display_session_list(ACTIVE_SESSIONS, COMPLETED_SESSIONS, PAUSED_SESSIONS, ABANDONED_SESSIONS, ACTIVE_SESSION_ID)
```

## CLAUDE CODE IMPLEMENTATION

When executing this command, follow these exact steps:

### 1. Discover All Sessions
```
Use LS tool to list directories in: .deep-research/sessions/
Filter out .current-research file
For each session directory, attempt to read metadata.json
Build list of valid sessions with their metadata
```

### 2. Identify Active Session
```
Use Read tool to check: .deep-research/.current-research
If file exists, extract active session ID
Mark active session in the display
```

### 3. Categorize and Sort Sessions
```
Group sessions by status: active, completed, paused, abandoned
Sort each group by lastUpdated timestamp (most recent first)
Calculate summary statistics for each category
```

### 4. Display Formatted List
```
Use exact format specified in "Display Format" section below
Include session counts, progress indicators, and key metrics
Show available actions for each session
```

## Description

This command provides a comprehensive list of all research sessions in your workspace, showing their status, progress, and key metrics. It helps you navigate between different research projects and understand your research history.

## Display Format

### Header Section
```
📋 RESEARCH SESSION DIRECTORY
════════════════════════════════════════════════════════════════════════════════

📊 Session Summary:
• Active: [X] sessions
• Completed: [X] sessions  
• Paused: [X] sessions
• Abandoned: [X] sessions
• Total: [X] sessions

🕐 Last Activity: [Most recent session update time]
💾 Storage Used: [Total storage across all sessions]
```

### Active Sessions Section
```
🔬 ACTIVE SESSIONS ([X] total)
────────────────────────────────────────────────────────────────────────────────

[★] [Session Title] 
    ID: [session-id]
    Question: [Truncated research question...]
    Phase: [X/4] [Phase Name] | Progress: [X]%
    Started: [X] days ago | Updated: [X] hours ago
    Sources: [X] collected | Findings: [Yes/No]
    Status: 🔄 Currently Active

[2] [Session Title]
    ID: [session-id]  
    Question: [Truncated research question...]
    Phase: [X/4] [Phase Name] | Progress: [X]%
    Started: [X] days ago | Updated: [X] hours ago
    Sources: [X] collected | Findings: [Yes/No]
    Status: ⏸️ Paused

[Note: ★ indicates currently active session]
```

### Completed Sessions Section
```
✅ COMPLETED SESSIONS ([X] total)
────────────────────────────────────────────────────────────────────────────────

[1] [Session Title]
    ID: [session-id]
    Question: [Truncated research question...]
    Completed: [X] days ago | Duration: [X]h [Y]m
    Sources: [X] collected | Citations: [X] total
    Report: [Available/Missing] | Size: [X] words
    Status: ✅ Research Complete

[2] [Session Title]
    ID: [session-id]
    Question: [Truncated research question...]
    Completed: [X] days ago | Duration: [X]h [Y]m
    Sources: [X] collected | Citations: [X] total
    Report: [Available/Missing] | Size: [X] words
    Status: ✅ Research Complete
```

### Paused Sessions Section
```
⏸️ PAUSED SESSIONS ([X] total)
────────────────────────────────────────────────────────────────────────────────

[1] [Session Title]
    ID: [session-id]
    Question: [Truncated research question...]
    Phase: [X/4] [Phase Name] | Progress: [X]%
    Paused: [X] days ago | Duration: [X]h [Y]m
    Sources: [X] collected | Last Action: [Action description]
    Status: ⏸️ Paused at [Phase Name]

[2] [Session Title]
    ID: [session-id]
    Question: [Truncated research question...]
    Phase: [X/4] [Phase Name] | Progress: [X]%
    Paused: [X] days ago | Duration: [X]h [Y]m
    Sources: [X] collected | Last Action: [Action description]
    Status: ⏸️ Paused at [Phase Name]
```

### Abandoned Sessions Section
```
❌ ABANDONED SESSIONS ([X] total)
────────────────────────────────────────────────────────────────────────────────

[1] [Session Title]
    ID: [session-id]
    Question: [Truncated research question...]
    Phase: [X/4] [Phase Name] | Progress: [X]%
    Abandoned: [X] days ago | Duration: [X]h [Y]m
    Sources: [X] collected | Reason: [Abandonment reason if available]
    Status: ❌ Abandoned

[2] [Session Title]
    ID: [session-id]
    Question: [Truncated research question...]
    Phase: [X/4] [Phase Name] | Progress: [X]%
    Abandoned: [X] days ago | Duration: [X]h [Y]m
    Sources: [X] collected | Reason: [Abandonment reason if available]
    Status: ❌ Abandoned
```

### Footer Section
```
────────────────────────────────────────────────────────────────────────────────

🔗 SESSION ACTIONS:
• Switch to session: Use session ID with appropriate command
• Resume paused session: `/research-status` after switching
• View session details: `/research-current` for active session
• Start new session: `/research-start [research question]`
• Archive old sessions: Use `/research-end` to finalize incomplete sessions

💡 TIPS:
• Sessions are automatically saved and can be resumed anytime
• Use descriptive research questions for better session organization
• Complete or abandon old sessions to keep workspace organized
• Active sessions are backed up automatically

════════════════════════════════════════════════════════════════════════════════
```

## Session Categories

### Active Sessions
**Definition**: Sessions currently being worked on
**Characteristics**:
- Status: "active" in metadata
- May be marked as current active session in .current-research
- Recent activity (updated within last 7 days)
- Incomplete research phases

**Display Priority**: Highest (shown first)
**Actions Available**:
- Switch to session (set as current)
- View detailed progress 
- Continue research workflow
- Pause or abandon session

### Completed Sessions  
**Definition**: Sessions with finalized research and reports
**Characteristics**:
- Status: "completed" in metadata
- All four phases completed
- Final report generated
- Research question fully addressed

**Display Priority**: Second
**Actions Available**:
- View final report
- Export research data
- Use as reference for new research
- Archive or delete if needed

### Paused Sessions
**Definition**: Sessions temporarily stopped but intended to resume
**Characteristics**:
- Status: "paused" in metadata
- Partial progress through phases
- Intended to be resumed later
- May have been current session previously

**Display Priority**: Third
**Actions Available**:
- Resume research (set as current)
- View progress made so far
- Continue from last checkpoint
- Convert to abandoned if no longer needed

### Abandoned Sessions
**Definition**: Sessions that were started but not completed
**Characteristics**:
- Status: "abandoned" in metadata
- Incomplete research
- No intention to resume
- May contain partial useful data

**Display Priority**: Lowest (shown last)
**Actions Available**:
- Review partial research
- Extract useful sources or insights
- Delete to clean up workspace
- Convert back to paused if needed

## Session List Display Rules

### Session Ordering
1. **Within Category**: Sort by lastUpdated timestamp (most recent first)
2. **Cross-Category**: Active → Completed → Paused → Abandoned
3. **Current Session**: Mark with ★ symbol if currently active
4. **Numbering**: Sequential numbering within each category

### Information Display
1. **Session Title**: Derived from research question (truncated if long)
2. **Session ID**: Full timestamp-based identifier
3. **Research Question**: First 80 characters with ellipsis if longer
4. **Phase Progress**: Current phase and percentage complete
5. **Time Metrics**: Start date, last update, total duration
6. **Research Metrics**: Source count, findings status, citations
7. **Status Indicators**: Clear visual status with emoji

### Progress Indicators
```
Phase Display Format:
- Phase 1/4 Planning (15% weight)
- Phase 2/4 Gathering (40% weight)  
- Phase 3/4 Analysis (30% weight)
- Phase 4/4 Report (15% weight)

Progress Calculation:
- Overall % = Weighted sum of phase completion
- Visual: Use progress bar or percentage
- Status: Show current phase and next steps
```

### Filtering Options

#### By Status
- `--active`: Show only active sessions
- `--completed`: Show only completed sessions
- `--paused`: Show only paused sessions
- `--abandoned`: Show only abandoned sessions

#### By Time Period
- `--recent`: Sessions updated in last 7 days
- `--week`: Sessions from last week
- `--month`: Sessions from last month
- `--older`: Sessions older than 30 days

#### By Progress
- `--planning`: Sessions in planning phase
- `--gathering`: Sessions in gathering phase
- `--analysis`: Sessions in analysis phase
- `--report`: Sessions in report phase

#### By Research Area
- `--topic [keyword]`: Sessions matching topic keyword
- `--question [text]`: Sessions with question containing text
- `--sources [min]`: Sessions with at least N sources

### Session Actions

#### For Active Sessions
1. **Switch to Session**: Make it the current active session
2. **View Details**: Show comprehensive session information
3. **Continue Research**: Resume research workflow
4. **Pause Session**: Change status to paused
5. **Abandon Session**: Mark as abandoned

#### For Completed Sessions
1. **View Report**: Display final research report
2. **Export Data**: Save research data for external use
3. **View Sources**: List all collected sources
4. **Reference Research**: Use findings for new research
5. **Archive Session**: Move to archive directory

#### For Paused Sessions
1. **Resume Session**: Set as current active session
2. **View Progress**: Show current research status
3. **Continue from Checkpoint**: Resume from last action
4. **Update Plan**: Modify research plan if needed
5. **Abandon Session**: Convert to abandoned status

#### For Abandoned Sessions
1. **Review Progress**: See what research was completed
2. **Extract Sources**: Use collected sources for other research
3. **Delete Session**: Remove from workspace
4. **Convert to Paused**: Change status to paused for potential resume
5. **Archive Session**: Move to archive directory

## Navigation

### Session Selection
```
To switch to a specific session:
1. Note the session ID from the list
2. Use: `/research-switch [session-id]`
3. Or use: `/research-current` to view detailed info
4. Or use: `/research-status` to continue workflow
```

### Quick Actions
```
Common navigation patterns:
• `/research-list --active` - Show only active sessions
• `/research-list --recent` - Show recently updated sessions
• `/research-start [question]` - Start new research session
• `/research-current` - View current session details
• `/research-status` - Continue current research workflow
```

### Session Management
```
Workspace organization:
• Keep 1-2 active sessions at most
• Complete or abandon old sessions regularly
• Use descriptive research questions for easy identification
• Archive completed sessions periodically
• Clean up abandoned sessions that won't be resumed
```

## Which session would you like to work with?
1. Continue with current active session (`/research-status`)
2. Switch to a different session (provide session ID)
3. Start a new research session (`/research-start [question]`)
4. View detailed info for a specific session (`/research-current`)
5. Just show me the list without action

## Error Handling

### Missing Files
- **Sessions Directory**: Create if doesn't exist, show empty list
- **Metadata Files**: Skip sessions with missing metadata, show warning
- **Corrupted Metadata**: Attempt to parse, show error for invalid sessions
- **Access Permissions**: Show permissions error and suggest fixes

### Display Issues
- **Long Session Lists**: Paginate or truncate with navigation options
- **Corrupted Data**: Show available information, mark corrupted fields
- **Missing Information**: Use fallback values, clearly mark unknowns
- **Formatting Problems**: Ensure readable output with fallback formatting

### State Conflicts
- **Multiple Active Sessions**: Show warning, offer to resolve
- **Orphaned Current Session**: Clear .current-research file, show warning
- **Inconsistent States**: Reconcile metadata with file system
- **Invalid Session IDs**: Skip invalid sessions, show warning

## Metrics and Statistics

### Session Count Metrics
- **Total Sessions**: Count of all sessions
- **Active Sessions**: Currently being worked on
- **Completion Rate**: Percentage of sessions completed
- **Average Duration**: Mean time from start to completion

### Research Output Metrics
- **Total Sources**: Sum of sources across all sessions
- **Total Citations**: Sum of citations across all sessions
- **Total Words**: Sum of words generated across all sessions
- **Research Areas**: Count of distinct research topics

### Time and Activity Metrics
- **Research Time**: Total time spent across all sessions
- **Recent Activity**: Sessions updated in last 7 days
- **Session Frequency**: Average sessions per month
- **Completion Time**: Average time to complete sessions

### Quality Metrics
- **Source Quality**: Distribution of Tier 1, 2, 3 sources
- **Research Depth**: Average sources per session
- **Citation Quality**: Properly formatted citations percentage
- **Research Coverage**: Completeness of research questions

## Important Rules

### Session Discovery
1. **Complete Scanning**: Check all directories in sessions folder
2. **Metadata Validation**: Verify each metadata.json file is valid
3. **Error Tolerance**: Skip corrupted sessions, continue processing
4. **Performance**: Use efficient file system operations

### Display Consistency
1. **Formatting Standards**: Use exact format specifications
2. **Information Completeness**: Show all available session information
3. **Status Accuracy**: Ensure status indicators match actual state
4. **Time Calculations**: Use consistent time calculation methods

### Session Management
1. **State Synchronization**: Ensure displayed state matches file system
2. **Action Availability**: Only show actions that are actually available
3. **Navigation Support**: Provide clear session switching instructions
4. **Workspace Organization**: Encourage good session management practices

### User Experience
1. **Clear Organization**: Group sessions logically by status
2. **Actionable Information**: Include specific next steps for each session
3. **Quick Access**: Provide efficient navigation to common actions
4. **Progress Visibility**: Make research progress immediately apparent