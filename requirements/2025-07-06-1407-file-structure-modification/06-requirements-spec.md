# Deep Research File Structure Modification Requirements

## Problem Statement

The current Deep Research system stores all research sessions in a centralized `sessions/` directory within the project root. This creates limitations for users who want to organize their research by topic, reuse sources across multiple research sessions, and maintain their research data in project-specific directories.

## Solution Overview

Modify the Deep Research system to use the user's current working directory as the research location, enabling:
- Directory-based research organization
- Source reuse across multiple research sessions
- User-managed source collections
- Multiple research sessions per directory with shared resources

## Functional Requirements

### FR1: User Directory-Based Research
- **FR1.1**: Research sessions must be created in the user's current working directory (where the `/research-start` command is executed)
- **FR1.2**: The system must not require users to specify a directory path - it should automatically use the current working directory
- **FR1.3**: Each directory can contain multiple research sessions with shared source collections

### FR2: Source Management and Reuse
- **FR2.1**: The system must automatically detect and use existing source files in the research directory
- **FR2.2**: Users must be able to add sources to the research directory outside of the Deep Research workflow
- **FR2.3**: Multiple research sessions in the same directory must share the same `sources/` subdirectory
- **FR2.4**: The system must validate and convert existing sources to the expected format when starting new sessions

### FR3: Session Management
- **FR3.1**: Multiple research sessions in the same directory must maintain separate metadata files
- **FR3.2**: Each session must have a unique identifier for tracking purposes
- **FR3.3**: The system must track the active research session per directory
- **FR3.4**: Session metadata must be stored in a `.deep-research` hidden directory within the user's working directory

### FR4: Directory Structure
- **FR4.1**: The system must automatically create the research directory structure when starting a new session
- **FR4.2**: The global state file (`.current-research`) must be stored in the user's directory
- **FR4.3**: The following directory structure must be created:
  ```
  [user-directory]/
  ├── .deep-research/              # Hidden directory for session metadata
  │   ├── .current-research        # Active session tracker
  │   └── sessions/                # Session metadata storage
  │       └── [session-id]/
  │           └── metadata.json
  ├── sources/                     # Shared source collection
  │   ├── source-001-*.md
  │   └── source-inventory.md
  ├── [session-id]-plan.md         # Session-specific files
  ├── [session-id]-findings.md
  └── [session-id]-report.html
  ```

## Technical Requirements

### TR1: File Path Modifications
- **TR1.1**: Replace hardcoded `sessions/` directory references with user's current working directory
- **TR1.2**: Update all session path construction logic in command files
- **TR1.3**: Modify global state file location from `sessions/.current-research` to `.deep-research/.current-research`

### TR2: Command File Updates
The following files require modification with specific changes:

#### TR2.1: `commands/research-start.md`
- **Lines 32, 47, 65, 77, 123, 174, 199**: Replace `SESSION_DIR = "sessions/" + SESSION_ID` with user directory logic
- Update session directory creation to use `.deep-research/sessions/[session-id]/`
- Add logic to detect and validate existing sources in `sources/` directory
- Implement source indexing for reuse functionality

#### TR2.2: `commands/research-status.md`
- **Lines 24, 36, 48**: Update to read `.deep-research/.current-research` instead of `sessions/.current-research`
- Modify session path construction to use user directory
- Update session discovery logic

#### TR2.3: `commands/research-current.md`
- **Lines 19, 34, 48**: Update session path references to use `.deep-research/` structure
- Modify file existence checks for new directory structure

#### TR2.4: `commands/research-list.md`
- **Lines 19, 26, 36**: Change session directory scanning from `sessions/` to `.deep-research/sessions/`
- Update metadata file path construction

#### TR2.5: `commands/research-end.md`
- **Lines 19, 30, 104**: Update session path references to use new structure
- Modify cleanup logic for `.deep-research/.current-research`

### TR3: Session Identifier Updates
- **TR3.1**: Maintain current session ID format: `YYYY-MM-DD-HHMM-topic-slug`
- **TR3.2**: Use session ID for session-specific files: `[session-id]-plan.md`, `[session-id]-findings.md`, `[session-id]-report.html`
- **TR3.3**: Store session metadata in `.deep-research/sessions/[session-id]/metadata.json`

### TR4: Source Management Logic
- **TR4.1**: Implement source detection and validation when starting new sessions
- **TR4.2**: Create source inventory management for shared source collections
- **TR4.3**: Ensure source format compatibility between manual additions and system-generated sources
- **TR4.4**: Maintain source numbering consistency across sessions

## Implementation Hints

### Path Construction Pattern
Replace current pattern:
```bash
SESSION_DIR = "sessions/" + SESSION_ID
```

With new pattern:
```bash
USER_DIR = $(pwd)
RESEARCH_META_DIR = "$USER_DIR/.deep-research"
SESSION_META_DIR = "$RESEARCH_META_DIR/sessions/$SESSION_ID"
SOURCES_DIR = "$USER_DIR/sources"
```

### Global State Management
Replace current:
```bash
echo "$SESSION_ID" > sessions/.current-research
```

With new:
```bash
mkdir -p "$USER_DIR/.deep-research"
echo "$SESSION_ID" > "$USER_DIR/.deep-research/.current-research"
```

### Session File Creation
Session-specific files should be created in user directory:
```bash
PLAN_FILE = "$USER_DIR/$SESSION_ID-plan.md"
FINDINGS_FILE = "$USER_DIR/$SESSION_ID-findings.md"
REPORT_FILE = "$USER_DIR/$SESSION_ID-report.html"
```

## Acceptance Criteria

### AC1: Directory Structure
- [ ] User can create research sessions in any directory by running `/research-start`
- [ ] `.deep-research` hidden directory is created for session metadata
- [ ] `sources/` directory is created and shared across sessions
- [ ] Session-specific files use session ID prefixes

### AC2: Source Reuse
- [ ] Existing sources in `sources/` directory are automatically detected
- [ ] Users can manually add sources to `sources/` directory
- [ ] New research sessions can access and use existing sources
- [ ] Source inventory is maintained across sessions

### AC3: Session Management
- [ ] Multiple research sessions can exist in the same directory
- [ ] Each session maintains separate metadata and session files
- [ ] Active session tracking works per directory
- [ ] All existing slash commands work with new structure

### AC4: Backward Compatibility
- [ ] Template system continues to work from any user directory
- [ ] All four research phases function correctly
- [ ] Session resumption works across directory changes
- [ ] Command workflow remains unchanged for users

## Assumptions

1. **Template Access**: The Deep Research templates in the project's `templates/` directory remain accessible from any user directory
2. **Command Availability**: All Deep Research slash commands are available system-wide regardless of user directory
3. **File Permissions**: Users have read/write permissions in their working directories
4. **Directory Persistence**: User working directories remain stable during research sessions
5. **Session Uniqueness**: Session IDs are unique enough to prevent conflicts in shared directories

## Risk Mitigation

1. **Path Resolution**: Implement absolute path handling to avoid issues with directory navigation
2. **State Management**: Use robust file locking or atomic operations for state file updates
3. **Source Validation**: Implement comprehensive source format validation to handle user-added files
4. **Error Handling**: Provide clear error messages for permission issues or invalid directory states
5. **Migration Path**: Consider providing a migration utility for users with existing research in the old `sessions/` structure

## Future Considerations

1. **Cross-Directory Session Management**: Potential for managing sessions across multiple directories
2. **Source Synchronization**: Ability to sync sources between different research directories
3. **Template Customization**: User-specific template customization per research directory
4. **Collaboration Features**: Shared research directories for team collaboration