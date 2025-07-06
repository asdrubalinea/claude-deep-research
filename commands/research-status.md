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
   - Read `.current-research` file in project root
   - If file doesn't exist or is empty:
     - Show message: "No active research session"
     - Suggest using `/research-start` to begin new research
     - List available research sessions with `/research-list`
     - Exit

2. **Load Session Data**
   - Parse `.current-research` to get session directory path
   - Read `metadata.json` from session directory for current phase and progress in parallel
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
  - Load `01-plan.md` if exists
  - Show research plan review prompt
  - Ask for approval to proceed to Phase 2
  - Update metadata on approval

- **If planning complete:**
  - Show plan summary
  - Transition to Phase 2 automatically

### Phase 2 - Information Gathering
- **If gathering in progress:**
  - Load `02-sources/source-inventory.md` if exists
  - Show current source count and gaps
  - Continue with next sub-question from research plan
  - ## ultrathink
    Source evaluation and search prioritization requires careful analysis to ensure comprehensive coverage while maintaining quality standards.
  - Present parallel WebSearch and WebFetch prompts for next sources (up to 5 concurrent)

- **If gathering complete:**
  - Show source collection summary
  - Transition to Phase 3 automatically

### Phase 3 - Analysis
- **If analysis in progress:**
  - Load all sources from `02-sources/` directory in parallel
  - Show current findings progress
  - Continue analysis from last documented finding
  - ## ultrathink
    Synthesizing findings from multiple sources requires deep analytical thinking to identify patterns, contradictions, and comprehensive insights.
  - Present analysis prompts for remaining sources

- **If analysis complete:**
  - Show findings summary
  - Transition to Phase 4 automatically

### Phase 4 - Report Generation
- **If report in progress:**
  - Load `03-findings.md` and source data
  - Show current report status
  - Continue report generation from last section
  - Present final report compilation prompts

- **If report complete:**
  - Show completion summary
  - Offer options: view report, start new research, archive session

## Phase Transitions

### Planning → Gathering
**Trigger:** Research plan approved by user
**Actions:**
1. Update metadata: `phase: 2, phase_name: "Information Gathering"`
2. Create `02-sources/` directory
3. Initialize `source-inventory.md`
4. Set `current_action: "collect_sources"`
5. Load first sub-question from research plan

### Gathering → Analysis
**Trigger:** Sufficient sources collected (minimum 10-15 sources)
**Actions:**
1. Update metadata: `phase: 3, phase_name: "Analysis"`
2. Set `current_action: "analyze_sources"`
3. Create `03-findings.md` template
4. Load all collected sources for analysis

### Analysis → Report
**Trigger:** Findings document complete with all sub-questions addressed
**Actions:**
1. Update metadata: `phase: 4, phase_name: "Report Generation"`
2. Set `current_action: "generate_report"`
3. Prepare report template with findings data
4. Load citation data for final report

### Report Complete
**Trigger:** Final HTML report generated and reviewed
**Actions:**
1. Update metadata: `phase: 5, phase_name: "Complete"`
2. Set `current_action: "session_complete"`
3. Archive session data
4. Clear `.current-research` file

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