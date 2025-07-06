# Expert Detail Answers

**Date**: 2025-07-05  
**Questions Answered**: 5/5

## Q1: Should the new system create a commands/ directory structure like claude-code-requirements-builder/commands/?
**Answer**: Yes
**Reasoning**: Follows proven pattern for slash command organization

## Q2: Should we maintain separate template files in prompts/ for internal use while exposing slash commands to users?
**Answer**: No
**Reasoning**: User indicated redundancy concerns - consolidate into commands only

## Q3: Should the system use JSON metadata files for session tracking like the requirements-builder's metadata.json structure?
**Answer**: Yes
**Reasoning**: Provides comprehensive state management and progress tracking
**Custom Design**: JSON format specifically designed for deep research workflow with phase tracking, source management, and quality metrics

## Q4: Should research sessions be stored in a new directory structure different from the current research-sessions/ approach?
**Answer**: Yes
**Reasoning**: User indicated no need for backward compatibility, allows for improved structure

## Q5: Should the system implement a global state file (.current-research) to track the active research session?
**Answer**: Yes
**Reasoning**: Enables resumable sessions and consistent state management across commands