---
description: List all research sessions and manage session selection
allowed-tools:
  - Read
  - LS
  - Bash
---

# List Research Sessions

Display all available research sessions and manage session selection.

## Implementation

### 1. Check for Sessions Directory
```
SESSIONS_DIR = ".deep-research/sessions"
if not directory_exists(SESSIONS_DIR):
    display("📭 No research sessions found")
    display("💡 Use /research-start [question] to create your first session")
    exit
```

### 2. Discover All Sessions
```
SESSION_DIRS = list_directories(SESSIONS_DIR)
if empty(SESSION_DIRS):
    display("📭 No research sessions found")
    display("💡 Use /research-start [question] to create your first session")
    exit

SESSIONS = []
for each session_dir in SESSION_DIRS:
    METADATA_FILE = SESSIONS_DIR + "/" + session_dir + "/metadata.json"
    if file_exists(METADATA_FILE):
        METADATA = parse_json(read_file(METADATA_FILE))
        SESSIONS.append(METADATA)
```

### 3. Display Sessions List
```
display("📋 RESEARCH SESSIONS")
display("=" * 50)

for i, session in enumerate(SESSIONS):
    display((i+1) + ". " + session.id)
    display("   Question: " + session.question)
    display("   Status: " + session.status + " | Phase: " + session.phase)
    display("   Started: " + format_date(session.started))
    display("   Sources: " + session.sourcesCollected)
    display("")
```

### 4. Check Active Session
```
ACTIVE_SESSION_FILE = ".deep-research/.current-research"
if file_exists(ACTIVE_SESSION_FILE):
    ACTIVE_SESSION = read_file(ACTIVE_SESSION_FILE).strip()
    if not empty(ACTIVE_SESSION):
        display("🔬 Active session: " + ACTIVE_SESSION)
        display("")
```

### 5. Session Management Options
```
display("📋 Available Actions:")
display("1. Continue active session (/research-status)")
display("2. Switch to different session")
display("3. Start new session (/research-start [question])")
display("4. View session details")

ACTION = prompt("Select action:", ["continue", "switch", "new", "details"], "continue")

switch ACTION:
    case "continue":
        if empty(ACTIVE_SESSION):
            display("No active session to continue")
        else:
            redirect_to_research_status()
    case "switch":
        switch_session()
    case "new":
        display("Use: /research-start [your research question]")
    case "details":
        show_session_details()
```

## Session Management Functions

### Switch Session
```
function switch_session():
    SESSION_CHOICE = prompt("Enter session number or ID:", SESSIONS)
    
    if is_number(SESSION_CHOICE):
        SELECTED_SESSION = SESSIONS[SESSION_CHOICE - 1]
    else:
        SELECTED_SESSION = find_session_by_id(SESSION_CHOICE, SESSIONS)
    
    if SELECTED_SESSION:
        write_file(".deep-research/.current-research", SELECTED_SESSION.id)
        display("✅ Switched to session: " + SELECTED_SESSION.id)
        display("▶️  Use /research-status to continue")
    else:
        display("❌ Session not found")
```

### Show Session Details
```
function show_session_details():
    SESSION_CHOICE = prompt("Enter session number or ID:", SESSIONS)
    
    if is_number(SESSION_CHOICE):
        SELECTED_SESSION = SESSIONS[SESSION_CHOICE - 1]
    else:
        SELECTED_SESSION = find_session_by_id(SESSION_CHOICE, SESSIONS)
    
    if SELECTED_SESSION:
        display_session_details(SELECTED_SESSION)
    else:
        display("❌ Session not found")
```

### Display Session Details
```
function display_session_details(session):
    display("🔬 SESSION DETAILS")
    display("=" * 40)
    display("ID: " + session.id)
    display("Question: " + session.question)
    display("Status: " + session.status)
    display("Phase: " + session.phase)
    display("Started: " + session.started)
    display("Sub-questions: " + session.subQuestions)
    display("Sources collected: " + session.sourcesCollected)
    
    # Check for files
    PLAN_FILE = session.files.plan
    FINDINGS_FILE = session.files.findings
    REPORT_FILE = session.files.report
    
    display("")
    display("📁 Files:")
    display("Plan: " + (file_exists(PLAN_FILE) ? "✅" : "❌") + " " + PLAN_FILE)
    display("Findings: " + (file_exists(FINDINGS_FILE) ? "✅" : "❌") + " " + FINDINGS_FILE)
    display("Report: " + (file_exists(REPORT_FILE) ? "✅" : "❌") + " " + REPORT_FILE)
```

## Status Indicators

### Session Status
- **active**: Currently being worked on
- **completed**: Research finished successfully
- **paused**: Temporarily stopped

### Phase Status
- **planning**: Creating research plan
- **gathering**: Collecting sources
- **analysis**: Analyzing sources and creating findings
- **report**: Generating final report

### File Status
- ✅ File exists
- ❌ File missing

## Error Handling

### Missing Metadata
```
if not file_exists(metadata_file):
    warn("Metadata missing for session: " + session_id)
    display("Skipping corrupted session")
    continue
```

### Corrupted JSON
```
try:
    METADATA = parse_json(read_file(metadata_file))
catch:
    warn("Corrupted metadata for session: " + session_id)
    display("Skipping corrupted session")
    continue
```

### Empty Sessions Directory
```
if empty(SESSION_DIRS):
    display("📭 No research sessions found")
    display("💡 Use /research-start [question] to create your first session")
    exit
```

## Implementation Notes

### Session Discovery
- Scan `.deep-research/sessions/` directory
- Load metadata.json from each session directory
- Skip sessions with missing or corrupted metadata
- Sort sessions by creation date (newest first)

### Active Session Handling
- Read active session from `.deep-research/.current-research`
- Highlight active session in list
- Provide quick continue option

### Session Selection
- Support selection by number (1, 2, 3, etc.)
- Support selection by session ID
- Validate selection before switching

### File Verification
- Check existence of plan, findings, and report files
- Display file status with clear indicators
- Note missing files for troubleshooting