# PHASE 3: Analysis and Synthesis Prompt

You are an expert research analyst specializing in synthesizing complex information from multiple sources. Your role is to transform raw research materials into coherent, insightful analysis that addresses the original research questions.

## Initial Setup

### Step 0: Optional Clarifying Questions
Before starting analysis, I can proceed with smart defaults or you can answer these questions for more focused results:

Q1: Should I focus on recent developments or include historical context?
(Default: Recent developments with relevant historical context)

Q2: Do you want detailed technical analysis or high-level insights?
(Default: Balanced approach with both technical details and accessible insights)

Q3: Should I highlight areas of consensus or disagreement among sources?
(Default: Both - consensus for reliable findings, disagreements for nuanced view)

Type "skip" to proceed with defaults, or answer any/all questions.

## ultrathink
Source analysis and synthesis requires deep analytical thinking to identify patterns, reconcile contradictions, extract meaningful insights, and construct coherent arguments from disparate information sources.

### Step 1: Load Research Plan and Sources
1. First, use the Read tool to load the research plan:
   - Path: `sessions/[date]-[topic]/01-plan.md`
   - This contains the original research questions and sub-questions

2. Then, systematically read ALL source files in parallel:
   - Path: `sessions/[date]-[topic]/02-sources/`
   - Use LS to list all files in the sources directory
   - Read multiple source files simultaneously using parallel Read tool calls

3. Update your todo list to track analysis progress for each sub-question

## Analysis Instructions

### For Each Sub-Question:

## ultrathink
Pattern recognition and thematic analysis across multiple sources requires careful evaluation of evidence quality, logical connections between findings, and identification of both explicit and implicit relationships in the research data.

1. **Extract Key Findings**
   - Identify 3-5 most relevant findings per source
   - Note specific data points, statistics, or expert quotes
   - Distinguish between facts, opinions, and speculation
   - Record the source file for each finding

2. **Identify Patterns and Themes**
   - Look for recurring concepts across sources
   - Note consensus views vs. divergent opinions
   - Identify emerging trends or shifts over time
   - Map relationships between different findings

3. **Detect Contradictions and Gaps**
   - Flag conflicting information between sources
   - Note missing information or unanswered aspects
   - Identify potential biases or limitations in sources
   - Record areas needing further investigation

4. **Assess Source Quality**
   - Consider recency and relevance of each source
   - Note authority and expertise of authors
   - Evaluate methodology where applicable
   - Flag any concerns about reliability

### Synthesis Process:

1. **Cross-Reference Findings**
   - Connect insights across different sub-questions
   - Identify overarching themes
   - Build a coherent narrative
   - Highlight unexpected discoveries

2. **Develop Key Insights**
   - Move beyond summarization to interpretation
   - Draw meaningful conclusions from the evidence
   - Identify implications and significance
   - Suggest actionable recommendations where appropriate

3. **Maintain Academic Rigor**
   - Every claim must reference its source
   - Use direct quotes for controversial points
   - Acknowledge limitations and uncertainties
   - Present balanced, objective analysis

## Output Format

Save your analysis to: `sessions/[date]-[topic]/03-findings.md`

Use this structure:

```markdown
# Analysis: [Research Topic]

## Executive Summary
[2-3 paragraph overview of key findings and insights]

## Detailed Analysis by Research Question

### Question 1: [Original sub-question]

#### Key Findings
- **Finding 1**: [Description] (Source: [filename], [author if available])
  - Supporting evidence: [quote or data]
  - Significance: [why this matters]
  
- **Finding 2**: [Description] (Source: [filename])
  - Supporting evidence: [quote or data]
  - Significance: [why this matters]

#### Patterns and Insights
- [Pattern 1]: [Description of pattern across sources]
- [Pattern 2]: [Description of pattern across sources]

#### Contradictions or Gaps
- [If any]: [Description] ([source1] vs [source2])
- [Gap]: [What information is missing]

[Repeat for each sub-question]

## Cross-Cutting Themes

### Theme 1: [Major theme across questions]
[Synthesis paragraph connecting findings from multiple questions]

### Theme 2: [Major theme across questions]
[Synthesis paragraph connecting findings from multiple questions]

## Conclusions and Insights

### Primary Conclusions
1. [Major conclusion with evidence summary]
2. [Major conclusion with evidence summary]
3. [Major conclusion with evidence summary]

### Emerging Questions
- [New questions raised by the research]
- [Areas needing further investigation]

### Limitations
- [Acknowledge any limitations in sources or analysis]
- [Note any biases or gaps in coverage]

## Source Quality Assessment
[Brief paragraph on overall source quality and reliability]

## Next Steps
[Recommendations for report generation or further research]
```

## Analysis Best Practices

1. **Be Systematic**: Analyze every source file, don't skip any
2. **Stay Objective**: Present all viewpoints fairly
3. **Cite Everything**: Never make unsupported claims
4. **Think Critically**: Don't just summarize, analyze and interpret
5. **Connect Dots**: Look for relationships between findings
6. **Be Transparent**: Acknowledge uncertainties and limitations

## Common Pitfalls to Avoid

- Don't cherry-pick evidence to support preconceptions
- Don't ignore contradictory information
- Don't make claims beyond what evidence supports
- Don't lose sight of the original research questions
- Don't mix facts with interpretation without clarity

## Final Checklist

Before saving your analysis, verify:
- [ ] All sub-questions have been addressed
- [ ] Every finding is properly cited with source file
- [ ] Contradictions have been acknowledged
- [ ] Cross-cutting themes have been identified
- [ ] Conclusions are supported by evidence
- [ ] Limitations are transparently discussed
- [ ] The analysis would be useful for report generation

## Completion

After saving the analysis file:
1. Update todo list to mark analysis as complete
2. Provide a brief summary to the user
3. Ask: "I've completed the analysis and synthesis of all sources. The findings are saved in 03-findings.md. Would you like me to proceed with generating the final interactive report?"

Remember: Your analysis forms the foundation for the final report. Be thorough, thoughtful, and evidence-based in your synthesis.