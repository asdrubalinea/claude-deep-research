# Claude Deep Research System

Transform Claude Code into a systematic research agent that conducts multi-phase research with proper citations and structured output.

## Quick Start

```bash
cp commands/* ~/.claude/commands
```

Use slash commands to manage your research:

```bash
/research-start What are the latest breakthroughs in quantum computing?
```

The system guides you through 4 phases with streamlined workflow and optional customization.

## Commands

| Command | Description |
|---------|-------------|
| `/research-start [question]` | Begin new research with automated planning |
| `/research-status` | Check progress and continue current research |
| `/research-list` | See all research sessions and switch between them |
| `/research-end` | Finalize and complete research |

## How It Works

### 🎯 Phase 1: Planning
- Breaks your question into 5-7 focused sub-questions
- Creates structured research plan with search strategies
- Optional customization for time focus, depth level, and format

### 🔍 Phase 2: Information Gathering
- Searches and saves 15-25 high-quality sources
- Evaluates source credibility (Tier 1-3 classification)
- Systematic coverage of all sub-questions

### 📊 Phase 3: Analysis
- Synthesizes findings from all collected sources
- Answers original research question comprehensively
- Identifies patterns, themes, and key insights

### 📝 Phase 4: Report Generation
- Creates interactive HTML report with proper citations
- Professional presentation with structured findings
- Maintains academic citation standards

## Features

✅ **Streamlined Workflow** - Simplified commands with clear progression
✅ **Resumable Sessions** - Continue research across conversations
✅ **Source Management** - Organized collection in shared `sources/` directory
✅ **Citation Tracking** - Academic-standard source attribution
✅ **Quality Control** - Human approval checkpoints between phases

## Output Structure

```
./
├── .deep-research/
│   ├── .current-research               # Active session tracker
│   └── sessions/[session-id]/         # Session metadata
│       └── metadata.json
├── sources/                           # Shared source collection
│   ├── source-001-*.md
│   └── source-inventory.md
├── [session-id]-plan.md              # Research plan
├── [session-id]-findings.md          # Analysis results
└── [session-id]-report.html          # Final report
```

## Example Research Topics

- Technology trends and assessments
- Scientific literature reviews
- Market analysis and competitor research
- Policy and regulatory analysis
- Historical investigations

## Tips

1. **Be specific** - "quantum computing breakthroughs in 2024" beats "quantum stuff"
2. **Use quick or custom setup** - Choose quick start for defaults or custom for tailored approach
3. **Review at checkpoints** - Approve plans before moving to information gathering
4. **Sessions are resumable** - Use `/research-status` to continue across conversations

---

For technical details, see [CLAUDE.md](CLAUDE.md).
