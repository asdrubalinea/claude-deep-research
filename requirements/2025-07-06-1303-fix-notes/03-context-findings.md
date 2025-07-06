# Context Findings: Deep Research System Prompt Improvements

## Executive Summary
Based on comprehensive analysis of the Deep Research system and Claude Code best practices, I've identified specific improvements needed to align the system with Anthropic's guidelines and the user's fix notes.

## Key Findings

### 1. Ultrathink Implementation Requirements

**Discovery**: The ultrathink keyword is a documented Claude Code feature that allocates additional thinking budget:
- `ultrathink`: 31,999 tokens
- `think hard/megathink`: 10,000 tokens  
- `think`: 4,000 tokens

**Implementation Locations**:
- **Phase 1 Planning** (`prompts/PHASE-1-PLANNING.md`): Add ultrathink for research question decomposition
- **Phase 2 Gathering** (`prompts/PHASE-2-GATHERING.md`): Add ultrathink for source evaluation and search strategy
- **Phase 3 Analysis** (`prompts/PHASE-3-ANALYSIS.md`): Add ultrathink for pattern recognition and synthesis
- **Research Start Command** (`commands/research-start.md`): Add ultrathink for initial planning

### 2. Parallel Search Implementation

**Current Issue**: Phase 2 processes sub-questions sequentially (lines 31-45)

**Solution Pattern**:
```markdown
Execute multiple tool calls in a single response:
- Batch WebSearch queries for all sub-questions
- Process up to 5 searches simultaneously
- Use Task tool with parallel agent invocations
```

**Specific Files**:
- `prompts/PHASE-2-GATHERING.md`: Restructure search workflow
- `commands/research-status.md`: Add parallel file operations

### 3. Clarifying Questions Integration

**Pattern from Requirements Builder**:
- Ask 5 targeted yes/no questions with smart defaults
- Questions before action, not after
- Context-aware questions based on research topic

**Implementation Points**:
- **Phase 1**: Questions about scope, timeframe, depth, format preferences
- **Phase 2**: Questions about source types, regions, languages, access levels
- **Phase 3**: Questions about analysis focus, comparison needs, presentation style
- **Phase 4**: Questions about audience, detail level, visual preferences

### 4. Command Metadata Structure

**Required Format** (per Claude Code best practices):
```yaml
---
description: Clear, concise description of command purpose
allowed-tools:
  - Read
  - Write
  - Bash
  - TodoWrite
  - WebSearch
  - WebFetch
  - Task
---
```

**Files Needing Updates**:
- All 5 command files in `commands/` directory
- Each needs YAML front matter with description and tool list

### 5. Prompt Length Optimization

**Finding**: User prioritizes precision over brevity
- Keep detailed instructions where they ensure accuracy
- Extract templates to separate files to reduce clutter
- Use structured sections for clarity

**Specific Extractions**:
- Move HTML template from `PHASE-4-REPORT.md` to `templates/report-template.html`
- Extract example formats to reference files
- Keep core instructions inline

## Technical Implementation Details

### File Modification Priority

#### Priority 1: Command Files (Add Metadata)
1. `commands/research-start.md` - Add description, allowed-tools, ultrathink
2. `commands/research-status.md` - Add metadata, parallel operations
3. `commands/research-current.md` - Add metadata, user interaction prompts
4. `commands/research-list.md` - Add metadata, parallel metadata reads
5. `commands/research-end.md` - Add metadata, parallel validation

#### Priority 2: Phase Prompts (Core Improvements)
1. `prompts/PHASE-1-PLANNING.md` - Ultrathink, clarifying questions
2. `prompts/PHASE-2-GATHERING.md` - Parallel searches, ultrathink, questions
3. `prompts/PHASE-3-ANALYSIS.md` - Parallel reads, ultrathink, questions
4. `prompts/PHASE-4-REPORT.md` - Extract template, add questions

#### Priority 3: Supporting Files
1. Update `CLAUDE.md` to document new features
2. Create examples showing parallel search patterns

### Patterns to Follow

#### Ultrathink Usage Pattern:
```markdown
## ultrathink
[Complex task description requiring deep reasoning]
This requires careful analysis to ensure comprehensive coverage and logical structure.
```

#### Parallel Tool Usage Pattern:
```markdown
Execute these searches simultaneously:
1. WebSearch for [query 1]
2. WebSearch for [query 2]
3. WebFetch for [specific URL]
Process all results in parallel for efficiency.
```

#### Clarifying Questions Pattern:
```markdown
## Initial Clarification
Before proceeding, I need to understand your specific needs:

Q1: [Specific yes/no question]?
Default: [Yes/No] (reasoning for default)
```

### Technical Constraints

1. **Rate Limiting**: Parallel searches must respect domain-specific limits
2. **Token Budget**: Ultrathink allocates up to 31,999 tokens - use judiciously
3. **Session State**: Maintain backward compatibility with metadata.json
4. **File Structure**: Preserve existing directory organization

### Integration Points

1. **TodoWrite Integration**: Update todo items to track clarification responses
2. **Metadata Tracking**: Store user preferences from clarifying questions
3. **Progress Indicators**: Show parallel operation status
4. **Error Handling**: Graceful degradation if parallel operations fail

## Recommendations

1. **Start with Command Metadata**: Quick wins that establish proper structure
2. **Implement Ultrathink Strategically**: Focus on high-complexity decision points
3. **Test Parallel Operations**: Verify performance improvements
4. **Document Changes**: Update CLAUDE.md with new capabilities
5. **Maintain User Control**: Ensure clarifying questions enhance, not hinder, workflow

## Next Steps

With this context analysis complete, I'm ready to ask detailed implementation questions based on deep understanding of the codebase and requirements.