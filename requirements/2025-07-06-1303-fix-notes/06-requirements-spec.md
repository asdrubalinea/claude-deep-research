# Requirements Specification: Deep Research System Prompt Improvements

## Problem Statement

The Claude Deep Research system needs prompt improvements to align with Claude Code best practices and enhance effectiveness. Current issues include:
- No use of ultrathink keyword for complex reasoning
- Sequential rather than parallel search operations
- Missing clarifying questions to understand user needs
- Lack of proper command metadata (descriptions, allowed-tools)
- Prompts could be more precise and follow best practices

## Solution Overview

Implement comprehensive prompt improvements across all phase prompts and command files to:
1. Add ultrathink keywords at complex decision points (automatic insertion)
2. Enable parallel search operations (up to 5 concurrent)
3. Include optional clarifying questions with smart defaults
4. Add proper metadata to all command files
5. Optimize prompts for precision while maintaining effectiveness

## Functional Requirements

### FR1: Ultrathink Integration
- **Automatic Insertion**: Commands must automatically add ultrathink at appropriate points
- **Decision Points**:
  - Phase 1: Research question decomposition
  - Phase 2: Source evaluation and search strategy
  - Phase 3: Pattern recognition and synthesis
  - Phase 4: Report structure decisions
- **Format**: Use `## ultrathink` section with clear task description

### FR2: Parallel Search Implementation
- **Concurrency Limit**: Maximum 5 simultaneous operations
- **Applicable Operations**:
  - WebSearch queries for multiple sub-questions
  - WebFetch for multiple sources
  - File reading operations in analysis phase
- **Error Handling**: Graceful degradation to sequential if parallel fails

### FR3: Clarifying Questions
- **Optional Nature**: Users can skip with smart defaults
- **Question Format**: Yes/no questions with default values and reasoning
- **Phase-Specific Questions**:
  - Phase 1: Scope, timeframe, depth, format preferences (5 questions)
  - Phase 2: Source types, regions, languages, access (4 questions)
  - Phase 3: Analysis focus, comparison needs (3 questions)
  - Phase 4: Audience, detail level, visuals (3 questions)

### FR4: Command Metadata
- **Required Fields**:
  - `description`: Clear, concise command purpose
  - `allowed-tools`: List of tools the command uses
- **Format**: YAML front matter with `---` delimiters
- **No Validation**: System trusts proper implementation

### FR5: Prompt Optimization
- **Priority**: Precision over brevity
- **Template Extraction**: Move large templates to separate files
- **Structure**: Clear sections with headers
- **Instructions**: Keep detailed where needed for accuracy

## Technical Requirements

### TR1: File Modifications

#### Command Files (Add metadata and ultrathink)
1. `commands/research-start.md`
   - Add YAML metadata
   - Insert ultrathink for planning decomposition
   - Include 5 optional clarifying questions

2. `commands/research-status.md`
   - Add YAML metadata
   - Implement parallel file operations
   - Auto-insert ultrathink based on phase

3. `commands/research-current.md`
   - Add YAML metadata
   - Parallelize metadata reading
   - Add "What would you like to do next?" prompt

4. `commands/research-list.md`
   - Add YAML metadata
   - Parallelize session metadata reads
   - Add session selection prompt

5. `commands/research-end.md`
   - Add YAML metadata
   - Parallelize validation checks
   - Streamline verbose sections

#### Phase Prompts (Core improvements)
1. `prompts/PHASE-1-PLANNING.md`
   - Add ultrathink section for decomposition
   - Insert 5 optional clarifying questions
   - Maintain current structure

2. `prompts/PHASE-2-GATHERING.md`
   - Implement parallel search pattern
   - Add ultrathink for source evaluation
   - Include 4 optional clarifying questions
   - Restructure search workflow for batching

3. `prompts/PHASE-3-ANALYSIS.md`
   - Add parallel file reading
   - Insert ultrathink for synthesis
   - Include 3 optional clarifying questions

4. `prompts/PHASE-4-REPORT.md`
   - Extract HTML template to separate file
   - Add 3 optional clarifying questions
   - Reference template file

### TR2: Implementation Patterns

#### Ultrathink Pattern:
```markdown
## ultrathink
Decomposing this research question into comprehensive sub-questions requires deep analytical thinking to ensure we cover all relevant aspects and maintain logical structure.
```

#### Parallel Search Pattern:
```markdown
I'll search for information on all sub-questions simultaneously for efficiency:

[Execute these searches in parallel - up to 5 at once]
1. WebSearch: "[query for sub-question 1]"
2. WebSearch: "[query for sub-question 2]"
3. WebSearch: "[query for sub-question 3]"
```

#### Clarifying Questions Pattern:
```markdown
## Optional Clarifying Questions
I can proceed with smart defaults, or you can answer these questions for more tailored results:

Q1: Do you need information from a specific time period?
(Default: No - will search all time periods)

[Wait for response or "skip" before continuing]
```

#### Command Metadata Pattern:
```yaml
---
description: Start a new deep research session with AI-powered planning
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

### TR3: Breaking Changes Allowed
- No backward compatibility required
- Can restructure prompts significantly
- May change workflow if it improves effectiveness

## Implementation Hints

### Search Optimization
- Use Task tool for parallel agent invocations
- Batch WebSearch calls in single tool response
- Monitor rate limits per domain

### Clarifying Questions Implementation
- Store responses in metadata.json
- Use TodoWrite to track question progress
- Provide clear skip instructions

### Ultrathink Usage
- Reserve for genuinely complex decisions
- Don't overuse (31,999 token allocation)
- Clear description of what needs deep thinking

### File Organization
- Keep template references clean
- Maintain session directory structure
- Update CLAUDE.md with new features

## Acceptance Criteria

1. **All command files have proper YAML metadata** with description and allowed-tools
2. **Ultrathink is automatically inserted** at identified decision points
3. **Phase 2 implements parallel searches** with up to 5 concurrent operations
4. **Each phase includes optional clarifying questions** with smart defaults
5. **Prompts maintain or improve precision** while following best practices
6. **HTML template is extracted** from Phase 4 prompt
7. **User can skip clarifying questions** and proceed with defaults
8. **System uses integrated search tools** effectively
9. **Commands provide clear descriptions** of their actions
10. **No validation overhead** for metadata checking

## Assumptions

1. Users prefer efficiency - optional questions with smart defaults
2. Rate limiting won't be an issue with 5 concurrent operations
3. Ultrathink improves quality without excessive slowdown
4. Breaking changes are acceptable for better system
5. Precision is more important than brevity in prompts