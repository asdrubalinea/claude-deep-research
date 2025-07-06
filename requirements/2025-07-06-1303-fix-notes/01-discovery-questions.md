# Discovery Questions for Deep Research System Prompt Improvements

Based on the codebase analysis, here are the five most important yes/no questions to understand the improvement requirements:

## Q1: Should the system ask users clarifying questions at the beginning of each research phase (not just at the end)?
**Default if unknown:** Yes (better to clarify user needs upfront to provide more accurate results)

## Q2: Will users prefer shorter, more concise prompts even if it means less detailed guidance?
**Default if unknown:** Yes (based on fix notes requesting shorter prompts and following best practices)

## Q3: Should the ultrathink keyword be used for all complex decision points (e.g., source evaluation, plan decomposition)?
**Default if unknown:** Yes (Claude Code best practices recommend ultrathink for complex reasoning)

## Q4: Do users expect the system to perform multiple searches in parallel when gathering information?
**Default if unknown:** Yes (fix notes specifically mention using subagents for parallel searches)

## Q5: Should command prompts (like /research-start) have explicit allowed-tools and descriptions metadata?
**Default if unknown:** Yes (Claude Code best practices require descriptions and allowed-tools lists)