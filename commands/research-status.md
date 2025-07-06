---
description: Continue research session and track progress through phases
allowed-tools:
  - Read
  - Write
  - Bash
  - TodoWrite
  - WebSearch
  - WebFetch
  - Task
---

# Research Status & Continue

Show current research session progress and continue workflow.

## Description

This command displays the status of your active research session, shows current progress through the four phases, and continues from where you left off. It provides a clear view of completed work and next steps.

## Implementation

### 1. Check for Active Session
```
ACTIVE_SESSION_FILE = ".deep-research/.current-research"
if not file_exists(ACTIVE_SESSION_FILE):
    display("❌ No active research session found")
    display("💡 Use /research-start [question] to begin new research")
    display("📋 Use /research-list to see all available sessions")
    exit
```

### 2. Load Session Data
```
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

### 3. Display Current Status
```
display("🔬 RESEARCH SESSION: " + SESSION_ID)
display("❓ Question: " + METADATA.question)
display("📊 Phase: " + METADATA.phase + " | Status: " + METADATA.status)
display("🕐 Started: " + METADATA.started)
display("📚 Sources collected: " + METADATA.sourcesCollected)
display("🎯 Sub-questions: " + METADATA.subQuestions)
```

### 4. Continue Based on Current Phase
```
switch METADATA.phase:
    case "planning":
        continue_planning_phase()
    case "gathering": 
        continue_gathering_phase()
    case "analysis":
        continue_analysis_phase()
    case "report":
        continue_report_phase()
    default:
        error("Unknown phase: " + METADATA.phase)
```

## Phase Implementations

### Planning Phase
```
function continue_planning_phase():
    PLAN_FILE = SESSION_ID + "-plan.md"
    if not file_exists(PLAN_FILE):
        display("⚠️  Research plan not found. Creating new plan...")
        execute_research_planning()
        write_file(PLAN_FILE, RESEARCH_PLAN)
        
    display("📋 Research plan exists: " + PLAN_FILE)
    APPROVAL = prompt("Review plan and proceed to information gathering?", ["yes", "review"], "yes")
    
    if APPROVAL == "yes":
        update_metadata({phase: "gathering"})
        update_todo_status("Information Gathering", "in_progress")
        display("▶️  Moving to information gathering phase")
        continue_gathering_phase()
    else:
        display("Plan ready for review. Run /research-status again when ready to proceed.")
```

### Gathering Phase  
```
function continue_gathering_phase():
    PLAN_FILE = SESSION_ID + "-plan.md"
    PLAN_CONTENT = read_file(PLAN_FILE)
    SUB_QUESTIONS = extract_sub_questions(PLAN_CONTENT)
    
    display("📚 Information Gathering Phase")
    display("📝 Sub-questions: " + count(SUB_QUESTIONS))
    
    SOURCE_COUNT = count_sources("sources/")
    display("📄 Sources collected: " + SOURCE_COUNT)
    
    if SOURCE_COUNT < 15:
        display("🔍 Collecting more sources...")
        collect_sources_for_questions(SUB_QUESTIONS)
    else:
        display("✅ Sufficient sources collected")
        PROCEED = prompt("Proceed to analysis phase?", ["yes", "collect_more"], "yes")
        
        if PROCEED == "yes":
            update_metadata({phase: "analysis"})
            update_todo_status("Analysis & Synthesis", "in_progress")
            continue_analysis_phase()
        else:
            collect_sources_for_questions(SUB_QUESTIONS)
```

### Analysis Phase
```
function continue_analysis_phase():
    SOURCES_DIR = "sources/"
    SOURCE_FILES = list_files(SOURCES_DIR, "*.md")
    
    display("🔍 Analysis & Synthesis Phase")
    display("📊 Analyzing " + count(SOURCE_FILES) + " sources")
    
    FINDINGS_FILE = SESSION_ID + "-findings.md"
    if not file_exists(FINDINGS_FILE):
        display("📝 Creating findings document...")
        analyze_sources_and_create_findings(SOURCE_FILES)
        write_file(FINDINGS_FILE, FINDINGS_CONTENT)
    
    display("✅ Analysis complete: " + FINDINGS_FILE)
    PROCEED = prompt("Proceed to report generation?", ["yes", "review"], "yes")
    
    if PROCEED == "yes":
        update_metadata({phase: "report"})
        update_todo_status("Report Generation", "in_progress")
        continue_report_phase()
```

### Report Phase
```
function continue_report_phase():
    FINDINGS_FILE = SESSION_ID + "-findings.md"
    REPORT_FILE = SESSION_ID + "-report.html"
    
    display("📄 Report Generation Phase")
    
    if not file_exists(REPORT_FILE):
        display("📝 Creating final report...")
        FINDINGS = read_file(FINDINGS_FILE)
        REPORT_CONTENT = generate_html_report(FINDINGS, METADATA)
        write_file(REPORT_FILE, REPORT_CONTENT)
    
    display("✅ Report generated: " + REPORT_FILE)
    display("🎉 Research session complete!")
    
    FINALIZE = prompt("Finalize session?", ["yes", "continue"], "yes")
    if FINALIZE == "yes":
        update_metadata({status: "completed"})
        update_todo_status("Report Generation", "completed")
        display("Session completed successfully!")
```

## Source Collection Process

### Collect Sources for Questions
```
function collect_sources_for_questions(SUB_QUESTIONS):
    for each question in SUB_QUESTIONS:
        display("🔍 Researching: " + question.text)
        
        SEARCH_RESULTS = web_search(question.search_strategy)
        PROMISING_SOURCES = filter_top_sources(SEARCH_RESULTS, 3)
        
        for each source in PROMISING_SOURCES:
            SOURCE_CONTENT = web_fetch(source.url)
            SOURCE_FILE = create_source_file(SOURCE_CONTENT, source)
            write_file("sources/" + SOURCE_FILE, SOURCE_CONTENT)
            
        increment_source_count(METADATA)
        
    display("📚 Source collection complete")
```

### Source File Creation
```
function create_source_file(content, source):
    SOURCE_ID = get_next_source_id()
    FILENAME = "source-" + pad_zeros(SOURCE_ID, 3) + "-" + 
               create_slug(source.title) + ".md"
    
    FORMATTED_CONTENT = format_source_content(content, source)
    return {filename: FILENAME, content: FORMATTED_CONTENT}
```

## Analysis Process

### Analyze Sources and Create Findings
```
function analyze_sources_and_create_findings(SOURCE_FILES):
    FINDINGS = {
        overview: "",
        key_findings: [],
        sub_question_answers: [],
        citations: []
    }
    
    for each source_file in SOURCE_FILES:
        SOURCE_CONTENT = read_file(source_file)
        ANALYSIS = analyze_source_content(SOURCE_CONTENT)
        FINDINGS.key_findings.append(ANALYSIS.insights)
        FINDINGS.citations.append(ANALYSIS.citation)
    
    FINDINGS.overview = synthesize_overview(FINDINGS.key_findings)
    FINDINGS.sub_question_answers = answer_sub_questions(FINDINGS.key_findings)
    
    return format_findings_document(FINDINGS)
```

## Report Generation

### Generate HTML Report
```
function generate_html_report(findings, metadata):
    REPORT_TEMPLATE = load_html_template()
    
    REPORT_DATA = {
        title: derive_title(metadata.question),
        question: metadata.question,
        session_id: metadata.id,
        date: current_date(),
        findings: findings,
        sources: load_source_inventory(),
        citations: extract_citations(findings)
    }
    
    return populate_template(REPORT_TEMPLATE, REPORT_DATA)
```

## Utility Functions

### Update Metadata
```
function update_metadata(updates):
    METADATA_FILE = ".deep-research/sessions/" + SESSION_ID + "/metadata.json"
    CURRENT_METADATA = parse_json(read_file(METADATA_FILE))
    
    for key, value in updates:
        CURRENT_METADATA[key] = value
    
    write_json(METADATA_FILE, CURRENT_METADATA)
```

### Update Todo Status
```
function update_todo_status(task_name, new_status):
    CURRENT_TODOS = read_todos()
    for todo in CURRENT_TODOS:
        if todo.content == task_name:
            todo.status = new_status
    write_todos(CURRENT_TODOS)
```

## Error Handling

### Missing Files
```
if not file_exists(required_file):
    warn("Missing file: " + required_file)
    offer_recovery_options()
```

### Corrupted Session
```
if invalid_metadata(METADATA):
    warn("Session metadata corrupted")
    attempt_recovery()
```

### Phase Mismatch
```
if phase_files_inconsistent():
    warn("Phase files inconsistent with metadata")
    reconcile_session_state()
```

## Progress Display

### Status Summary
```
display("=" * 60)
display("RESEARCH SESSION STATUS")
display("=" * 60)
display("Session: " + SESSION_ID)
display("Question: " + METADATA.question)
display("Phase: " + METADATA.phase + " (" + calculate_progress() + "% complete)")
display("Sources: " + METADATA.sourcesCollected)
display("Started: " + format_date(METADATA.started))
display("=" * 60)
```

### Phase Progress
```
PHASES = ["planning", "gathering", "analysis", "report"]
CURRENT_PHASE_INDEX = PHASES.index(METADATA.phase)

for i, phase in enumerate(PHASES):
    if i < CURRENT_PHASE_INDEX:
        display("✅ " + phase.capitalize())
    elif i == CURRENT_PHASE_INDEX:
        display("⏳ " + phase.capitalize() + " (in progress)")
    else:
        display("⏸️  " + phase.capitalize() + " (pending)")
```

## Next Steps Display

### Action Recommendations
```
display("🎯 NEXT STEPS:")
switch METADATA.phase:
    case "planning":
        display("1. Review research plan")
        display("2. Approve and proceed to gathering")
    case "gathering":
        display("1. Continue collecting sources")
        display("2. Aim for 15-25 high-quality sources")
    case "analysis":
        display("1. Analyze collected sources")
        display("2. Create findings document")
    case "report":
        display("1. Generate final report")
        display("2. Review and finalize session")
```