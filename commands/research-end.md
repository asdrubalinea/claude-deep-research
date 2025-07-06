---
description: Finalize and complete the active research session
allowed-tools:
  - Read
  - Write
  - Bash
  - TodoWrite
---

# End Research Session

Finalize and complete the active research session.

## Implementation

### 1. Validate Active Session
```
ACTIVE_SESSION_FILE = ".deep-research/.current-research"
if not file_exists(ACTIVE_SESSION_FILE):
    display("❌ No active research session found")
    display("💡 Use /research-start [question] to begin new research")
    exit

SESSION_ID = read_file(ACTIVE_SESSION_FILE).strip()
if empty(SESSION_ID):
    error("No active session ID found")
    exit

METADATA_FILE = ".deep-research/sessions/" + SESSION_ID + "/metadata.json"
if not file_exists(METADATA_FILE):
    error("Session metadata not found for: " + SESSION_ID)
    exit

METADATA = parse_json(read_file(METADATA_FILE))
```

### 2. Display Session Summary
```
display("🔬 FINALIZING RESEARCH SESSION")
display("=" * 50)
display("Session ID: " + SESSION_ID)
display("Question: " + METADATA.question)
display("Phase: " + METADATA.phase)
display("Status: " + METADATA.status)
display("Sources collected: " + METADATA.sourcesCollected)
display("Started: " + METADATA.started)
display("")
```

### 3. Check Session Completeness
```
PLAN_FILE = METADATA.files.plan
FINDINGS_FILE = METADATA.files.findings
REPORT_FILE = METADATA.files.report

display("📁 File Status:")
display("Plan: " + (file_exists(PLAN_FILE) ? "✅" : "❌") + " " + PLAN_FILE)
display("Findings: " + (file_exists(FINDINGS_FILE) ? "✅" : "❌") + " " + FINDINGS_FILE)
display("Report: " + (file_exists(REPORT_FILE) ? "✅" : "❌") + " " + REPORT_FILE)
display("")

COMPLETION_STATUS = calculate_completion_status(METADATA.phase, file_statuses)
display("📊 Completion: " + COMPLETION_STATUS)
```

### 4. Finalization Options
```
if METADATA.phase == "report" and file_exists(REPORT_FILE):
    display("✅ Research appears complete!")
    ACTION = prompt("Action:", ["finalize", "continue", "cancel"], "finalize")
else:
    display("⚠️  Research appears incomplete")
    ACTION = prompt("Action:", ["finalize_anyway", "continue", "cancel"], "continue")

switch ACTION:
    case "finalize" or "finalize_anyway":
        finalize_session()
    case "continue":
        display("Returning to research. Use /research-status to continue.")
        exit
    case "cancel":
        display("Finalization cancelled.")
        exit
```

### 5. Finalize Session
```
function finalize_session():
    # Update session status
    FINAL_METADATA = METADATA
    FINAL_METADATA.status = "completed"
    FINAL_METADATA.completedAt = current_iso_timestamp()
    
    write_json(METADATA_FILE, FINAL_METADATA)
    
    # Clear active session
    delete_file(ACTIVE_SESSION_FILE)
    
    # Update todos
    complete_all_research_todos()
    
    # Display completion summary
    display_completion_summary(SESSION_ID, FINAL_METADATA)
```

## Completion Functions

### Calculate Completion Status
```
function calculate_completion_status(phase, file_statuses):
    PHASE_WEIGHTS = {
        "planning": 25,
        "gathering": 50, 
        "analysis": 75,
        "report": 100
    }
    
    BASE_COMPLETION = PHASE_WEIGHTS[phase] or 0
    
    # Adjust based on file existence
    if file_exists(PLAN_FILE):
        BASE_COMPLETION = max(BASE_COMPLETION, 25)
    if file_exists(FINDINGS_FILE):
        BASE_COMPLETION = max(BASE_COMPLETION, 75)
    if file_exists(REPORT_FILE):
        BASE_COMPLETION = 100
        
    return BASE_COMPLETION + "% complete"
```

### Complete All Research Todos
```
function complete_all_research_todos():
    CURRENT_TODOS = read_todos()
    RESEARCH_TODOS = filter_research_todos(CURRENT_TODOS)
    
    for todo in RESEARCH_TODOS:
        todo.status = "completed"
    
    write_todos(CURRENT_TODOS)
```

### Display Completion Summary
```
function display_completion_summary(session_id, metadata):
    display("🎉 RESEARCH SESSION COMPLETED!")
    display("=" * 50)
    display("Session: " + session_id)
    display("Question: " + metadata.question)
    display("Duration: " + calculate_duration(metadata.started, metadata.completedAt))
    display("Sources collected: " + metadata.sourcesCollected)
    display("")
    
    display("📁 Generated Files:")
    if file_exists(metadata.files.plan):
        display("📋 Research Plan: " + metadata.files.plan)
    if file_exists(metadata.files.findings):
        display("📊 Research Findings: " + metadata.files.findings)
    if file_exists(metadata.files.report):
        display("📄 Final Report: " + metadata.files.report)
    
    display("")
    display("✅ Session finalized and saved")
    display("💡 Use /research-list to view all sessions")
    display("💡 Use /research-start [question] to begin new research")
```

## Utility Functions

### Calculate Duration
```
function calculate_duration(start_time, end_time):
    DURATION_MS = parse_timestamp(end_time) - parse_timestamp(start_time)
    HOURS = floor(DURATION_MS / 3600000)
    MINUTES = floor((DURATION_MS % 3600000) / 60000)
    
    if HOURS > 0:
        return HOURS + "h " + MINUTES + "m"
    else:
        return MINUTES + "m"
```

### Filter Research Todos
```
function filter_research_todos(todos):
    RESEARCH_KEYWORDS = ["Research", "Information", "Analysis", "Report", "Planning"]
    FILTERED = []
    
    for todo in todos:
        for keyword in RESEARCH_KEYWORDS:
            if contains(todo.content, keyword):
                FILTERED.append(todo)
                break
    
    return FILTERED
```

## Validation and Safety

### Pre-Finalization Checks
```
# Warn if session seems incomplete
if METADATA.phase != "report":
    display("⚠️  Warning: Session is in '" + METADATA.phase + "' phase")
    display("    Consider completing the research before finalizing")

# Warn if critical files missing
if not file_exists(PLAN_FILE):
    display("⚠️  Warning: Research plan file missing")
if METADATA.sourcesCollected < 5:
    display("⚠️  Warning: Only " + METADATA.sourcesCollected + " sources collected")
```

### Backup Session Data
```
function backup_session_data(session_id):
    BACKUP_DIR = ".deep-research/backups/" + session_id
    create_directory(BACKUP_DIR)
    
    # Copy all session files to backup
    copy_file(METADATA_FILE, BACKUP_DIR + "/metadata.json")
    if file_exists(PLAN_FILE):
        copy_file(PLAN_FILE, BACKUP_DIR + "/" + basename(PLAN_FILE))
    if file_exists(FINDINGS_FILE):
        copy_file(FINDINGS_FILE, BACKUP_DIR + "/" + basename(FINDINGS_FILE))
    if file_exists(REPORT_FILE):
        copy_file(REPORT_FILE, BACKUP_DIR + "/" + basename(REPORT_FILE))
```

### Recovery Information
```
function display_recovery_info():
    display("")
    display("🔧 Recovery Information:")
    display("If you need to resume this session:")
    display("1. Restore active session: echo '" + SESSION_ID + "' > .deep-research/.current-research")
    display("2. Update metadata status to 'active'")
    display("3. Use /research-status to continue")
```

## Error Handling

### Missing Session Files
```
if not file_exists(METADATA_FILE):
    error("Cannot finalize: Session metadata missing")
    display("Session may be corrupted or already finalized")
    exit
```

### Invalid Session State
```
if METADATA.status == "completed":
    display("⚠️  Session already completed on: " + METADATA.completedAt)
    RECONFIRM = prompt("Finalize again?", ["yes", "no"], "no")
    if RECONFIRM != "yes":
        exit
```

### File System Errors
```
try:
    write_json(METADATA_FILE, FINAL_METADATA)
catch:
    error("Failed to update session metadata")
    display("Session finalization failed - please try again")
    exit
```

## Implementation Notes

### Session State Management
- Update metadata status to "completed"
- Add completion timestamp
- Clear active session reference
- Complete related todo items

### File Preservation
- Never delete research files during finalization
- Maintain all source files and generated documents
- Keep session metadata for future reference

### User Experience
- Provide clear completion summary
- Show generated files and their locations
- Offer guidance for next steps
- Include recovery information if needed