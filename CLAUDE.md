# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Deep Research System that transforms Claude Code into an advanced research agent using a four-phase workflow with human checkpoints. The system now features **slash commands** for streamlined workflow management, eliminating the need for manual prompt copying.

### Four-Phase Research Methodology:
1. **Phase 1**: Research Planning - Decompose complex questions into actionable sub-questions
2. **Phase 2**: Information Gathering - Systematically search and save sources
3. **Phase 3**: Analysis - Analyze sources and create findings
4. **Phase 4**: Report Generation - Create interactive HTML reports with citations

### Key Improvements:
- **Command-driven interface** - No more manual prompt copying
- **Session management** - Resumable research across multiple conversations
- **Progress tracking** - Comprehensive metadata and state management
- **Quality assurance** - Automated source evaluation and citation management

## Common Commands

### Research Workflow Commands
The Deep Research System now uses slash commands for streamlined workflow management:

1. **`/research-start [your research question]`** - Initiates a new research session with automated planning
2. **`/research-status`** - Checks progress and continues from where you left off
3. **`/research-current`** - Shows detailed view of your active research session
4. **`/research-list`** - Lists all research sessions (active, completed, paused)
5. **`/research-end`** - Finalizes and completes your research session

### Legacy Manual Process (Deprecated)
For reference, the old manual process involved copying prompts from:
- `prompts/PHASE-1-PLANNING.md`, `prompts/PHASE-2-GATHERING.md`, etc.
- This is no longer recommended - use the slash commands instead

### File Management
- Research sessions are saved in `sessions/YYYY-MM-DD-HHMM-topic-slug/`
- Metadata tracking via `metadata.json` in each session directory
- Global state management with `sessions/.current-research` file
- Use TodoWrite to track progress through the four phases (automated by commands)
- Save artifacts using Claude's artifact feature for final reports

## Architecture and File Structure

### Core System Components

```
deep-research/
├── commands/                   # Slash command implementations
│   ├── research-start.md      # /research-start command
│   ├── research-status.md     # /research-status command
│   ├── research-current.md    # /research-current command
│   ├── research-list.md       # /research-list command
│   └── research-end.md        # /research-end command
├── sessions/                   # Research session storage
│   ├── .current-research      # Global state file
│   └── YYYY-MM-DD-HHMM-slug/  # Individual research sessions
│       ├── metadata.json      # Session state and progress
│       ├── 01-plan.md         # Research plan
│       ├── 02-sources/        # Source collection
│       │   ├── source-001-*.md
│       │   └── source-inventory.md
│       ├── 03-findings.md     # Analysis results
│       └── 04-report.html     # Final report
├── templates/                  # Internal templates for generation
│   ├── plan-template.md       # Research plan template
│   ├── source-template.md     # Source file template
│   ├── findings-template.md   # Analysis template
│   └── report-template.html   # Report template
├── prompts/                    # Legacy prompt files (reference only)
├── requirements/              # System requirements documentation
└── CLAUDE.md                  # This file
```

### Key Architectural Patterns

1. **Phase-Based Workflow**: Each phase has a dedicated prompt template that sets Claude's role and provides specific instructions. Human checkpoints between phases ensure quality control.

2. **Structured Output**: All research outputs follow consistent naming conventions and directory structures to maintain organization across multiple research sessions.

3. **Web Content Fetching**: Uses WebFetch tool for gathering online sources, with ethical guidelines for respecting robots.txt, rate limits, and copyright.

4. **Citation Management**: Strict citation standards ensure all claims are verifiable with proper attribution following academic formatting.

5. **Multi-Modal Synthesis**: System can analyze text, images, charts, and data from various sources to create comprehensive reports.

## Important Implementation Details

### When Starting Research
1. Use `/research-start [your research question]` to automatically:
   - Create session directory structure: `sessions/YYYY-MM-DD-HHMM-topic-slug/`
   - Generate metadata.json for state tracking
   - Execute Phase 1 planning with sub-question decomposition
   - Set up TodoWrite to track the four phases
   - Save the research plan before proceeding to Phase 2
2. Approve the research plan when prompted
3. Use `/research-status` to continue to Phase 2 (Information Gathering)

### During Information Gathering (Phase 2)
- Use `/research-status` to continue systematic source collection
- System automatically creates `02-sources/` subdirectory
- Sources are named as `source-NNN-descriptive-title.md` (e.g., source-001-openai-announcement.md)
- System maintains `source-inventory.md` file with quality tiers and coverage assessment
- Respect ethical web research guidelines from `RESEARCH-GUIDELINES.md`
- Progress tracked in metadata.json (sources collected vs. targeted)

### For Analysis (Phase 3)
- Use `/research-status` to transition to analysis phase
- System automatically reads all sources from `02-sources/` directory
- Creates comprehensive findings document at `03-findings.md`
- Includes proper citations for all claims
- Analyzes each sub-question systematically
- Identifies cross-cutting themes and patterns

### When Generating Reports (Phase 4)
- Use `/research-status` to transition to report generation
- System generates interactive HTML report using professional template
- Includes all citations with proper formatting
- Creates presentation-ready output with:
  - Executive summary and key findings
  - Detailed analysis by sub-question
  - Cross-cutting themes and insights
  - Comprehensive citation list
  - Research methodology and limitations
- Use `/research-end` to finalize and complete the session

### Quality Standards
- Follow source evaluation tiers (Tier 1: peer-reviewed journals, government data; Tier 2: professional publications; Tier 3: use with caution)
- Implement cross-referencing for controversial claims
- Present multiple perspectives on disputed topics
- Acknowledge limitations and gaps in available information

## Research Guidelines Summary

### Ethical Principles
- **Respect robots.txt** and website terms of service
- **Avoid rapid requests** to the same domain
- **Provide proper attribution** for all sources
- **Do not bypass paywalls** or access restrictions
- **Focus on publicly available** information

### Citation Format
```
[Author Last, First]. (Year). "Article Title." *Publication Name*. URL. Accessed: [Date].
```

### Handling Edge Cases
- **Conflicting sources**: Document all viewpoints and note credibility
- **Information gaps**: Acknowledge limitations and expand search if needed  
- **Biased sources**: Identify bias and seek contrasting perspectives
- **Access limitations**: Note what couldn't be accessed and why