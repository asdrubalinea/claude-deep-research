# Expert Detail Questions

**Phase**: Expert Questions  
**Date**: 2025-07-05  
**Total Questions**: 5

## Q1: Should the new system create a commands/ directory structure like claude-code-requirements-builder/commands/?
**Default if unknown:** Yes (follows proven pattern for slash command organization)

## Q2: Should we maintain separate template files in prompts/ for internal use while exposing slash commands to users?
**Default if unknown:** Yes (keeps existing prompt templates while adding user-friendly commands)

## Q3: Should the system use JSON metadata files for session tracking like the requirements-builder's metadata.json structure?
**Default if unknown:** Yes (provides comprehensive state management and progress tracking)

## Q4: Should research sessions be stored in a new directory structure different from the current research-sessions/ approach?
**Default if unknown:** Yes (user indicated no need for backward compatibility, allows for improved structure)

## Q5: Should the system implement a global state file (.current-research) to track the active research session?
**Default if unknown:** Yes (enables resumable sessions and consistent state management across commands)