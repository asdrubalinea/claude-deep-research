# Expert Technical Questions

## Q1: Should the system use a `.deep-research` hidden directory in the user's working directory for session metadata?
**Default if unknown:** Yes (keeps user directory clean while providing dedicated space for session tracking)

## Q2: Should the global state file (.current-research) be stored in the user's directory or system-wide location?
**Default if unknown:** User's directory (each directory can have its own active research session)

## Q3: Should the system automatically create the research directory structure (sources/, findings/, etc.) when starting a new session?
**Default if unknown:** Yes (maintains current organized structure that users expect)

## Q4: Should the system validate and index existing source files when starting a new session in a directory with pre-existing files?
**Default if unknown:** Yes (essential for source reuse functionality as specified in requirements)

## Q5: Should multiple research sessions in the same directory share the same sources/ subdirectory or maintain separate source collections?
**Default if unknown:** Share the same sources/ subdirectory (enables source reuse across multiple research sessions as requested)