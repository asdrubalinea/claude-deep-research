# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is the Claude Deep Research System - a sophisticated research framework that transforms Claude Code into an advanced research agent capable of conducting systematic, multi-phase research with proper citations and structured output.

## System Architecture

### Core Components

**Slash Commands Structure**: The system operates through 5 slash commands located in the `commands/` directory:
- `research-start.md` - Initiates new research sessions with automated planning
- `research-status.md` - Continues current research and checks progress  
- `research-current.md` - Displays detailed active session information
- `research-list.md` - Lists all research sessions
- `research-end.md` - Finalizes and completes research

**Four-Phase Research Methodology**:
1. **Planning Phase**: Breaks research questions into 5-7 focused sub-questions
2. **Information Gathering Phase**: Collects 15-25 high-quality sources with tier classification
3. **Analysis Phase**: Synthesizes findings from all sources
4. **Report Generation Phase**: Creates interactive HTML reports with citations

### Session Management

**Session Structure**: Each research session creates:
```
.deep-research/sessions/[SESSION_ID]/metadata.json  # Session state tracking
[SESSION_ID]-plan.md                               # Research plan
[SESSION_ID]-findings.md                           # Analysis results  
[SESSION_ID]-report.html                           # Final report
sources/                                           # Shared source collection
```

**Session Identification**: Uses format `YYYY-MM-DD-HHMM-topic-slug` for unique session IDs.

**Global State**: Managed through `.deep-research/.current-research` file that tracks the active session.

## Development Commands

**Installation**: Copy commands to Claude Code directory:
```bash
cp commands/* ~/.claude/commands
```

**Usage**: Research sessions are initiated with:
```bash
/research-start [your research question]
```

## Key Implementation Details

### Metadata Management
- Sessions tracked via comprehensive JSON metadata in `.deep-research/sessions/`
- Progress tracking through four-phase status system
- Source inventory with tier classification (Tier 1-3 quality levels)

### Source Collection Strategy
- Systematic search using WebSearch and WebFetch tools
- Source validation with credibility assessment
- Proper citation formatting and reference management

### Quality Control
- Human checkpoints between each phase
- Clarifying questions with smart defaults
- User approval required before phase transitions

### File Organization
- Consistent naming conventions with session ID prefixes
- Structured directory hierarchy
- Resumable sessions across conversations

## Important Notes

- This system requires no build processes, tests, or compilation
- All functionality is delivered through Claude Code slash commands
- Sessions are stateful and resumable
- The system emphasizes research quality over speed
- Proper academic citation standards are maintained throughout

## Research Topics Supported

- Technology trends and assessments
- Scientific literature reviews  
- Market analysis and competitor research
- Policy and regulatory analysis
- Historical investigations
- Complex multi-faceted research questions

## Session Workflow

1. User initiates research with specific question
2. System asks optional clarifying questions (can be skipped)
3. Planning phase creates structured research plan
4. Information gathering phase collects quality sources
5. Analysis phase synthesizes findings
6. Report generation creates final deliverable
7. Session can be paused/resumed at any time