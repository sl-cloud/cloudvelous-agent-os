# Workflow Methods Reference

> **Detailed documentation for all pipeline methods**

---

## Overview

This reference provides detailed technical documentation for each workflow method available in the Workflow Lab.

---

## Decomposition Methods

### Calculate Complexity Metrics

**Method ID:** `calculate_complexity_metrics`

**Risk Level:** Low

**Estimated Duration:** ~0 seconds

**Supported Modes:** sandbox, live

#### Description

Analyzes requirement text and calculates complexity metrics including:
- Character count
- Section count
- Bullet point count
- Other structural indicators

#### Use Cases

- Determine if a requirement needs decomposition
- Estimate processing time for a requirement
- Sort requirements by complexity for prioritization

#### Inputs

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| requirement_text | string | Yes | The requirement text to analyze |

#### Outputs

Returns a complexity score object with:
- `char_count` - Total characters
- `section_count` - Number of sections/paragraphs
- `bullet_count` - Number of bullet points
- `complexity_score` - Calculated complexity rating

---

### Decompose Requirement (Full Pipeline)

**Method ID:** `decompose_requirement_full_pipeline`

**Risk Level:** Medium

**Estimated Duration:** ~30 seconds

**Supported Modes:** sandbox

#### Description

Full decomposition pipeline that:
1. Analyzes complexity
2. Uses LLM to decompose into logical chunks
3. Generates .md files for each chunk in output directory

#### Use Cases

- Breaking down large requirements
- Creating manageable work units
- Generating chunked documentation

#### Inputs

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| project_name | string | Yes | Project containing the requirement |
| requirement_id | string | Yes | Requirement to decompose |
| output_directory | string | Yes | Where to write chunk files |

#### Outputs

- Multiple .md files in the output directory
- Decomposition summary with chunk list
- Complexity analysis results

---

## Planning Methods

### Extract Plan Markdown

**Method ID:** `extract_plan_markdown`

**Risk Level:** Low

**Estimated Duration:** ~0 seconds

**Supported Modes:** sandbox, live

#### Description

Extracts and normalizes plan markdown from agent output. Handles various formatting inconsistencies.

#### Use Cases

- Parsing planning output from LLM agents
- Normalizing different markdown formats
- Extracting structured plans from unstructured text

#### Inputs

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| agent_output | string | Yes | Raw text from agent |
| format_type | string | No | Target format (default: standard) |

#### Outputs

- Normalized markdown plan
- Structured plan object
- Extracted sections and tasks

---

## Refinement Methods

### Sanitize Agent Output

**Method ID:** `sanitize_agent_output`

**Risk Level:** Low

**Estimated Duration:** ~0 seconds

**Supported Modes:** sandbox, live

#### Description

Removes CLI noise and transcript artifacts from agent output, including:
- Command prompt artifacts
- Transcript markers
- Debug output
- ANSI color codes

#### Use Cases

- Cleaning agent responses for further processing
- Removing transcript formatting
- Preparing output for storage

#### Inputs

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| raw_output | string | Yes | Raw agent output |
| remove_ansi | boolean | No | Strip color codes (default: true) |

#### Outputs

- Cleaned text content
- List of removed artifacts

---

## Review Methods

### Parse Review Concerns

**Method ID:** `parse_review_concerns`

**Risk Level:** Low

**Estimated Duration:** ~0 seconds

**Supported Modes:** sandbox, live

#### Description

Extracts structured concerns from review markdown. Converts free-form review text into actionable items.

#### Use Cases

- Converting review comments into tasks
- Extracting action items from code reviews
- Structuring feedback for developers

#### Inputs

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| review_markdown | string | Yes | Review text in markdown |

#### Outputs

- Array of concern objects:
  - `severity` - critical, major, minor
  - `category` - code, test, doc, etc.
  - `description` - Concern description
  - `suggestion` - Recommended fix

---

### Review Cycle Trace Runner

**Method ID:** `workflow_lab_review_cycle`

**Risk Level:** Medium

**Estimated Duration:** ~60 seconds

**Supported Modes:** sandbox

#### Description

Executes one full review cycle with xdebug-like trace visibility. Provides detailed step-by-step execution tracing.

#### Use Cases

- Debugging review workflows
- Understanding review cycle behavior
- Testing review configurations
- Validating review logic

#### Inputs

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| project_name | string | Yes | Project containing the requirement |
| requirement_id | string | Yes | Requirement to review |
| start_mode | enum | No | `latest` or `specific_iteration` (default: latest) |
| start_iteration_number | number | No | Specific iteration if start_mode=specific |
| max_cycles | number | No | Number of review cycles (1-5, default: 1) |
| continue_from_run_id | string | No | Resume from previous run |

#### Outputs

- Full execution trace
- Review decisions at each step
- Producer and reviewer outputs
- Final review state

---

## Utility Methods

### Validate File Path

**Method ID:** `validate_file_path`

**Risk Level:** Low

**Estimated Duration:** ~0 seconds

**Supported Modes:** sandbox, live

#### Description

Validates a file path against security rules:
- Symlink traversal detection
- Path traversal prevention
- Allowed directory validation

#### Use Cases

- Security validation before file operations
- Path sanitization
- Preventing directory escape attacks

#### Inputs

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| file_path | string | Yes | Path to validate |
| allowed_base | string | No | Base directory that must contain path |

#### Outputs

- `valid` - Boolean validity result
- `normalized_path` - Cleaned path
- `error` - Error message if invalid

---

### Resolve Artifact Path

**Method ID:** `resolve_artifact_path`

**Risk Level:** Low

**Estimated Duration:** ~0 seconds

**Supported Modes:** sandbox, live

#### Description

Resolves the filesystem path for a requirement artifact based on project and artifact configuration.

#### Use Cases

- Finding where to read/write artifacts
- Path resolution for requirements
- Artifact organization

#### Inputs

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| project_name | string | Yes | Project identifier |
| requirement_id | string | Yes | Requirement identifier |
| artifact_type | string | Yes | Type of artifact (plan, code, test, etc.) |

#### Outputs

- `full_path` - Resolved filesystem path
- `exists` - Whether path currently exists
- `directory` - Parent directory path

---

### Write Artifact File

**Method ID:** `write_artifact_file`

**Risk Level:** Medium

**Estimated Duration:** ~0 seconds

**Supported Modes:** sandbox, live

#### Description

Writes content to an artifact file with security validation. Combines path validation with file writing.

#### Use Cases

- Saving generated code
- Writing test files
- Storing documentation

#### Inputs

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| project_name | string | Yes | Project identifier |
| requirement_id | string | Yes | Requirement identifier |
| artifact_type | string | Yes | Type of artifact |
| content | string | Yes | Content to write |
| overwrite | boolean | No | Allow overwriting existing files |

#### Outputs

- `success` - Boolean write result
- `path` - Path where file was written
- `bytes_written` - Size of written content

---

## Method Quick Reference

### By Risk Level

| Low Risk | Medium Risk |
|----------|-------------|
| Calculate Complexity Metrics | Decompose Requirement |
| Extract Plan Markdown | Review Cycle Trace Runner |
| Sanitize Agent Output | Write Artifact File |
| Parse Review Concerns | |
| Validate File Path | |
| Resolve Artifact Path | |

### By Phase

| Decomposition | Planning | Refinement | Review | Utils |
|---------------|----------|------------|--------|-------|
| Calculate Complexity | Extract Plan Markdown | Sanitize Agent Output | Parse Review Concerns | Validate File Path |
| Decompose Requirement | | | Review Cycle Trace Runner | Resolve Artifact Path |
| | | | | Write Artifact File |

### By Duration

| Quick (<1s) | Medium (~30s) | Long (~60s) |
|-------------|---------------|-------------|
| Calculate Complexity | Decompose Requirement | Review Cycle Trace Runner |
| Extract Plan | | |
| Sanitize Output | | |
| Parse Concerns | | |
| Validate Path | | |
| Resolve Path | | |
| Write File | | |

---

## Common Patterns

### Decomposition Pattern

```
1. Calculate Complexity Metrics → Determine if decomposition needed
2. If complex → Decompose Requirement (Full Pipeline)
3. Process each chunk independently
```

### Review Pattern

```
1. Parse Review Concerns → Extract actionable items
2. For each concern → Create task for developer
3. Track until resolved
```

### File Operation Pattern

```
1. Validate File Path → Security check
2. If valid → Resolve Artifact Path → Get full path
3. Write Artifact File → Persist content
```

---

*Part of the Cloudvelous Engineering Workflow Documentation*
