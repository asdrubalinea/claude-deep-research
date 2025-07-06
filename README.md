# Claude Deep Research System

Transform Claude Code into an advanced research agent that conducts systematic, multi-phase research with proper citations.

## Quick Start

Use slash commands to manage your research:

```bash
/research-start What are the latest breakthroughs in quantum computing?
```

The system will guide you through 4 phases with optional clarifying questions at each step.

## Commands

| Command | Description |
|---------|-------------|
| `/research-start [question]` | Begin new research with automated planning |
| `/research-status` | Check progress and continue current research |
| `/research-current` | View detailed info about active session |
| `/research-list` | See all research sessions |
| `/research-end` | Finalize and complete research |

## How It Works

### 🎯 Phase 1: Planning
- Breaks your question into focused sub-questions
- **Optional questions**: Time period, technical depth, source priorities
- Creates structured research plan

### 🔍 Phase 2: Information Gathering  
- Searches and saves 15-25 high-quality sources
- **Optional questions**: Academic vs industry sources, regional focus
- Evaluates source credibility (Tier 1-3)

### 📊 Phase 3: Analysis
- Synthesizes findings from all sources
- **Optional questions**: Recent vs historical focus, technical detail level
- Identifies patterns and themes

### 📝 Phase 4: Report Generation
- Creates interactive HTML report with citations
- **Optional questions**: Audience type, practical vs theoretical emphasis
- Professional presentation format

## Features

✅ **Smart Defaults** - Skip clarifying questions to use sensible defaults  
✅ **Resumable Sessions** - Continue research across conversations  
✅ **Parallel Operations** - Fast concurrent searches and analysis  
✅ **Citation Management** - Academic-standard source attribution  
✅ **Quality Control** - Human checkpoints between phases  

## Output Structure

```
sessions/
└── 2025-07-06-1245-quantum-computing/
    ├── metadata.json          # Session state
    ├── 01-plan.md            # Research questions
    ├── 02-sources/           # Saved sources
    ├── 03-findings.md        # Analysis
    └── 04-report.html        # Final report
```

## Example Research Topics

- Technology trends and assessments
- Scientific literature reviews  
- Market analysis and competitor research
- Policy and regulatory analysis
- Historical investigations

## Tips

1. **Be specific** - "quantum computing breakthroughs in 2024" beats "quantum stuff"
2. **Review checkpoints** - Take time to approve plans and findings
3. **Trust the process** - Let the system handle the complexity

---

For technical details, see [CLAUDE.md](CLAUDE.md).