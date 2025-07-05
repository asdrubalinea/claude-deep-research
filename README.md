# Claude Code Deep Research System - Quick Start Guide

## Overview
This system transforms Claude Code into an advanced research agent using a four-phase workflow with human checkpoints.

## Starting Your First Research

### Phase 1: Planning
1. Open Claude Code in the deep-research directory
2. Copy the content from `prompts/PHASE-1-PLANNING.md`
3. Add your research question at the end
4. Claude will create a research plan and ask for approval

Example:
```
[Paste PHASE-1-PLANNING.md content]

Research Question: "What are the most effective strategies for reducing carbon emissions in urban transportation systems?"
```

### Phase 2: Information Gathering
After approving the plan:
1. Copy the content from `prompts/PHASE-2-GATHERING.md`
2. Claude will systematically search and save sources
3. Review the source inventory when complete

### Phase 3: Analysis
After reviewing sources:
1. Copy the content from `prompts/PHASE-3-ANALYSIS.md`
2. Claude will analyze all sources and create findings
3. Review the analysis document

### Phase 4: Final Report
To generate the report:
1. Copy the content from `prompts/PHASE-4-REPORT.md`
2. Claude will create an interactive HTML report as an artifact
3. The report includes all findings with proper citations

## File Organization

Your research will be saved in:
```
research-sessions/
└── YYYY-MM-DD-topic-name/
    ├── 01-research-plan.md     # Your decomposed questions
    ├── 02-sources/             # All fetched content
    │   ├── source-001-title.md
    │   ├── source-002-title.md
    │   └── source-inventory.md
    ├── 03-findings.md          # Analysis results
    └── 04-report.html          # Final report (if saved)
```

## Tips for Success

1. **Be Specific**: Clear, focused research questions yield better results
2. **Review Checkpoints**: Take time to review and modify at each phase
3. **Source Quality**: During Phase 2, you can request additional sources if needed
4. **Iterative Process**: You can return to any phase if needed
5. **Save Artifacts**: Use Claude's artifact save feature for the final report

## Common Commands

- **Check Progress**: Ask "What's the current research status?"
- **Add Sources**: "Find more sources about [specific aspect]"
- **Modify Plan**: Edit the research plan before proceeding to Phase 2
- **Resume Work**: "Continue research from [phase name]"

## Best Practices

1. **Research Questions**: Frame as open-ended but specific queries
2. **Source Diversity**: Aim for varied perspectives and source types
3. **Time Management**: Each phase typically takes 5-10 minutes
4. **Documentation**: Save important artifacts for future reference

## Troubleshooting

- **Rate Limits**: If searches fail, wait a moment and retry
- **Lost Context**: Reference the saved files to restore state
- **Unclear Results**: Request clarification or additional analysis
- **Technical Issues**: Check the CLAUDE.md file for detailed guidance

## Example Research Topics

- Technology impact assessments
- Market analysis and trends
- Scientific literature reviews
- Policy research and analysis
- Historical investigations
- Comparative studies

Start your research journey by pasting the Phase 1 prompt!