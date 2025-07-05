# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Deep Research System that transforms Claude Code into an advanced research agent using a four-phase workflow with human checkpoints. The system guides structured research through:

1. **Phase 1**: Research Planning - Decompose complex questions into actionable sub-questions
2. **Phase 2**: Information Gathering - Systematically search and save sources
3. **Phase 3**: Analysis - Analyze sources and create findings
4. **Phase 4**: Report Generation - Create interactive HTML reports with citations

## Common Commands

### Research Workflow Commands
There are no build, test, or lint commands for this documentation-focused repository. Instead, the key commands are:

1. **Start new research**: Paste content from `prompts/PHASE-1-PLANNING.md` with your research question
2. **Continue to gathering**: Paste content from `prompts/PHASE-2-GATHERING.md` 
3. **Move to analysis**: Paste content from `prompts/PHASE-3-ANALYSIS.md`
4. **Generate report**: Paste content from `prompts/PHASE-4-REPORT.md`

### File Management
- Research sessions are saved in `research-sessions/YYYY-MM-DD-topic-name/`
- Use TodoWrite to track progress through the four phases
- Save artifacts using Claude's artifact feature for final reports

## Architecture and File Structure

### Core System Components

```
deep-research/
├── prompts/                    # Phase-specific prompt templates
│   ├── PHASE-1-PLANNING.md    # Research decomposition prompt
│   ├── PHASE-2-GATHERING.md   # Source collection prompt  
│   ├── PHASE-3-ANALYSIS.md    # Analysis and synthesis prompt
│   ├── PHASE-4-REPORT.md      # Report generation prompt
│   └── RESEARCH-GUIDELINES.md # Ethical guidelines and standards
├── research-sessions/          # Output directory for all research
│   └── YYYY-MM-DD-topic/      # Individual research projects
│       ├── 01-research-plan.md
│       ├── 02-sources/
│       ├── 03-findings.md
│       └── 04-report.html
├── templates/                  # Templates for outputs
└── requirements/              # System requirements documentation
```

### Key Architectural Patterns

1. **Phase-Based Workflow**: Each phase has a dedicated prompt template that sets Claude's role and provides specific instructions. Human checkpoints between phases ensure quality control.

2. **Structured Output**: All research outputs follow consistent naming conventions and directory structures to maintain organization across multiple research sessions.

3. **Web Content Fetching**: Uses WebFetch tool for gathering online sources, with ethical guidelines for respecting robots.txt, rate limits, and copyright.

4. **Citation Management**: Strict citation standards ensure all claims are verifiable with proper attribution following academic formatting.

5. **Multi-Modal Synthesis**: System can analyze text, images, charts, and data from various sources to create comprehensive reports.

## Important Implementation Details

### When Starting Research
1. Always create the research directory structure: `research-sessions/YYYY-MM-DD-topic-name/`
2. Use TodoWrite to track the four phases
3. Save the research plan before proceeding to Phase 2

### During Information Gathering (Phase 2)
- Create subdirectory `02-sources/` for storing fetched content
- Name source files as `source-NNN-descriptive-title.md` (e.g., source-001-openai-announcement.md)
- Maintain a `source-inventory.md` file listing all sources
- Respect ethical web research guidelines from `RESEARCH-GUIDELINES.md`

### For Analysis (Phase 3)
- Read all previously saved sources from the `02-sources/` directory
- Create comprehensive findings document at `03-findings.md`
- Include proper citations for all claims

### When Generating Reports (Phase 4)
- Generate as an interactive HTML artifact when possible
- Include all citations with proper formatting
- Create professional, presentation-ready output

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