# Requirements Specification v2: Direct Claude Code Deep Research System

## 1. Problem Statement and Solution Overview

### Problem
The user needs a custom research system that leverages Claude Code directly through carefully crafted prompts and multiple conversation steps, avoiding external orchestration while maintaining human oversight throughout the research process.

### Solution Overview
Create a prompt-based research workflow that uses Claude Code's native capabilities (WebSearch, WebFetch, file operations) across multiple conversation phases. Each phase will have specific prompt templates that guide Claude through the research process, with natural human checkpoints between phases.

## 2. Functional Requirements

### 2.1 Core Research Workflow
- **FR-1**: System shall use distinct prompts for each research phase
- **FR-2**: System shall save intermediate results as files for phase continuity
- **FR-3**: System shall allow human review between each phase
- **FR-4**: System shall generate interactive HTML reports using Claude Artifacts
- **FR-5**: System shall maintain research context through saved files

### 2.2 Research Phases
- **FR-6**: Phase 1 - Research Planning: Decompose query into sub-questions
- **FR-7**: Phase 2 - Information Gathering: Use WebSearch and WebFetch
- **FR-8**: Phase 3 - Analysis: Synthesize findings from saved sources
- **FR-9**: Phase 4 - Report Generation: Create final artifact
- **FR-10**: Each phase shall have a dedicated prompt template

### 2.3 Data Management
- **FR-11**: System shall save all research data as markdown files
- **FR-12**: System shall organize files by research session
- **FR-13**: System shall maintain source citations throughout
- **FR-14**: System shall cache web content to avoid re-fetching
- **FR-15**: System shall support MCP for local file access (optional)

### 2.4 Human Controls
- **FR-16**: Human reviews research plan before execution
- **FR-17**: Human can modify sub-questions between phases
- **FR-18**: Human approves sources before deep analysis
- **FR-19**: Human reviews draft before final report
- **FR-20**: Human can request iterations at any phase

## 3. Technical Requirements

### 3.1 File Structure
```
deep-research/
├── prompts/
│   ├── PHASE-1-PLANNING.md      # Research planning prompt
│   ├── PHASE-2-GATHERING.md     # Information gathering prompt
│   ├── PHASE-3-ANALYSIS.md      # Analysis and synthesis prompt
│   ├── PHASE-4-REPORT.md        # Final report generation prompt
│   └── RESEARCH-GUIDELINES.md   # General instructions for all phases
├── research-sessions/
│   └── YYYY-MM-DD-topic/       # Per-session directory
│       ├── 01-research-plan.md  # Decomposed questions
│       ├── 02-sources/          # Cached web content
│       ├── 03-findings.md       # Analyzed findings
│       └── 04-report.html       # Final artifact
├── templates/
│   ├── research-plan-template.md
│   └── report-template.html
└── CLAUDE.md                    # Instructions for Claude Code
```

### 3.2 Claude Code Native Tools Usage
- **TR-1**: WebSearch for discovering relevant sources
- **TR-2**: WebFetch for extracting content from specific URLs
- **TR-3**: Read/Write for maintaining research state
- **TR-4**: TodoWrite for tracking research progress
- **TR-5**: Claude Artifacts for final report generation

### 3.3 Prompt Engineering Patterns
```markdown
# Phase 1 Planning Prompt Structure
You are an expert research strategist using Claude Code for deep research.
Your task is to create a comprehensive research plan.

Given the query: [USER_QUERY]

1. Break down into 5-7 specific sub-questions
2. Identify search strategies for each
3. Suggest source types (academic, news, technical docs)
4. Save plan to: research-sessions/[date]-[topic]/01-research-plan.md

Format as:
## Research Plan for: [TOPIC]
### Sub-Questions:
1. [Question] - Strategy: [search approach] - Sources: [types]
...

DO NOT begin searching yet. Focus only on planning.
```

### 3.4 Phase Transitions
- **TR-6**: Each phase ends with saved state file
- **TR-7**: Next phase begins by reading previous state
- **TR-8**: Human approval required between phases
- **TR-9**: Progress tracked via TodoWrite tool
- **TR-10**: Clear instructions for next steps provided

## 4. Implementation Workflow

### 4.1 Phase 1: Research Planning
```
User: [Provides research question]
Claude: 
- Creates research plan with sub-questions
- Saves to 01-research-plan.md
- Uses TodoWrite to track progress
- Asks: "Shall I proceed with this research plan?"
```

### 4.2 Phase 2: Information Gathering
```
User: "Yes, proceed" or provides modifications
Claude:
- Reads 01-research-plan.md
- For each sub-question:
  - Uses WebSearch to find sources
  - Uses WebFetch to extract content
  - Saves to 02-sources/
- Creates source summary
- Asks: "Ready to analyze these sources?"
```

### 4.3 Phase 3: Analysis & Synthesis
```
User: "Yes, analyze" or requests more sources
Claude:
- Reads all content from 02-sources/
- Analyzes findings for each sub-question
- Synthesizes connections and insights
- Saves to 03-findings.md
- Asks: "Shall I generate the final report?"
```

### 4.4 Phase 4: Report Generation
```
User: "Yes, create report" or requests changes
Claude:
- Reads 03-findings.md
- Generates interactive HTML artifact
- Includes all citations
- Professional formatting
- Presents as Claude Artifact
```

## 5. Prompt Templates

### 5.1 PHASE-1-PLANNING.md
```markdown
You are an expert research strategist. Your role is to decompose complex research questions into actionable sub-questions WITHOUT conducting any actual research yet.

Instructions:
1. Analyze the user's research question
2. Break it down into 5-7 specific, answerable sub-questions
3. For each sub-question, identify:
   - Best search strategies
   - Types of sources to prioritize
   - Potential challenges
4. Create a structured research plan
5. Save the plan using Write tool
6. Use TodoWrite to track progress

Remember: This phase is ONLY for planning. Do not search or analyze yet.
```

### 5.2 PHASE-2-GATHERING.md
```markdown
You are a thorough research assistant. Your role is to gather information systematically based on the approved research plan.

Instructions:
1. Read the research plan from 01-research-plan.md
2. For each sub-question:
   - Use WebSearch to find 3-5 relevant sources
   - Use WebFetch to extract full content
   - Save each source with clear naming
3. Track all sources with citations
4. Create a source inventory file
5. Update todo list as you progress

Focus on: Public sources, recent information, authoritative sites
Avoid: Paywalled content, suspicious domains, duplicate information
```

### 5.3 PHASE-3-ANALYSIS.md
```markdown
You are an expert analyst. Your role is to synthesize findings from gathered sources into coherent insights.

Instructions:
1. Read all content from 02-sources/
2. For each sub-question:
   - Extract key findings
   - Note contradictions or gaps
   - Identify patterns
3. Synthesize overall insights
4. Maintain citation tracking
5. Save analysis to 03-findings.md

Analysis should be: Objective, evidence-based, clearly structured
Include: Direct quotes, statistical data, expert opinions
```

### 5.4 PHASE-4-REPORT.md
```markdown
You are a professional report writer. Your role is to create a comprehensive, interactive research report.

Instructions:
1. Read the analysis from 03-findings.md
2. Create an HTML artifact with:
   - Executive summary
   - Detailed findings by topic
   - Interactive elements (collapsible sections)
   - Professional CSS styling
   - Complete bibliography
3. Ensure all claims have citations
4. Use clear, accessible language

The report should be suitable for: [Specify audience]
Tone: Professional, objective, informative
```

## 6. CLAUDE.md Configuration
```markdown
# Claude Code Configuration for Deep Research System

## Overview
This workspace contains a prompt-based deep research system that leverages Claude Code's native capabilities.

## Key Commands
- Start new research: Begin with PHASE-1-PLANNING.md prompt
- Continue research: Load appropriate phase prompt
- Check progress: Use TodoRead tool

## Important Patterns
1. Always save state between phases
2. Use WebSearch before WebFetch
3. Cache all fetched content locally
4. Maintain citation tracking throughout
5. Update todo list regularly

## File Naming Conventions
- Research sessions: YYYY-MM-DD-topic/
- Sources: 02-sources/[domain]-[title].md
- Always use descriptive filenames

## Quality Checks
- Verify sources are accessible
- Check for recent publication dates
- Ensure balanced perspective
- Validate citation accuracy
```

## 7. Acceptance Criteria

### 7.1 Research Quality
- [ ] Generates comprehensive research plans with 5-7 sub-questions
- [ ] Gathers at least 15-20 diverse sources per research topic
- [ ] Produces well-structured analysis with clear citations
- [ ] Creates professional HTML reports via Artifacts
- [ ] Maintains research continuity across phases

### 7.2 Process Flow
- [ ] Each phase has clear beginning and end
- [ ] Human can intervene at any checkpoint
- [ ] State is preserved between conversations
- [ ] Progress is tracked via todo system
- [ ] Clear instructions for phase transitions

### 7.3 Output Quality
- [ ] All sources are properly cited
- [ ] Reports are interactive and well-formatted
- [ ] Findings are objective and balanced
- [ ] Analysis connects multiple sources
- [ ] Executive summaries are concise

## 8. Example Research Flow

```
User: "Research the impact of AI on software development productivity"

Claude (Phase 1): 
*Uses PHASE-1-PLANNING.md prompt*
*Creates research plan with sub-questions like:*
- How is AI currently being used in software development?
- What productivity metrics show improvement?
- What are the limitations and challenges?
*Saves plan and asks for approval*

User: "Looks good, proceed"

Claude (Phase 2):
*Uses PHASE-2-GATHERING.md prompt*
*Searches and fetches content for each sub-question*
*Saves sources and creates inventory*
*Shows summary of sources found*

User: "Great sources, please analyze"

Claude (Phase 3):
*Uses PHASE-3-ANALYSIS.md prompt*
*Reads all sources and synthesizes findings*
*Creates structured analysis document*
*Highlights key insights and patterns*

User: "Generate the final report"

Claude (Phase 4):
*Uses PHASE-4-REPORT.md prompt*
*Creates interactive HTML artifact*
*Includes all findings with citations*
*Presents professional report*
```

## 9. Benefits of This Approach

1. **No External Dependencies**: Pure Claude Code implementation
2. **Full Transparency**: Human sees every step
3. **Flexible Intervention**: Can modify at any phase
4. **Natural Checkpoints**: Conversation breaks provide review points
5. **Easy Customization**: Just modify prompt templates
6. **Leverages Claude's Strengths**: Conversational AI with tool use
7. **Persistent State**: Files maintain context across sessions

This specification provides a complete blueprint for implementing a deep research system using only Claude Code's native capabilities and carefully crafted prompts.