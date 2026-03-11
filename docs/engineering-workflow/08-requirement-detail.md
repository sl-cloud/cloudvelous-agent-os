# 08 - Requirement Detail Page

> **Deep dive into individual requirements with iteration timeline and feedback decisions**

---

## Screenshot

![Requirement Detail Page](./screenshots/07-requirement-detail.png)

## Overview

The Requirement Detail page provides comprehensive visibility into a single requirement's lifecycle, including its decomposition, review cycles, consensus status, and iteration timeline with detailed feedback decisions.

---

## Purpose

The Requirement Detail module serves as:
- **Requirement Lifecycle View** - Complete history from creation to completion
- **Review Cycle Tracker** - Track iterations through the review process
- **Consensus Monitor** - View agent consensus status and voting
- **Decision Audit Trail** - Detailed feedback decisions with reasoning
- **TDD Workflow Control** - Start and manage TDD workflows

---

## Page Structure

### Header Section

```
┌─────────────────────────────────────────────────────────────────────────────┐
│ ← Back to Project                                                           │
│                                                                             │
│ req-20260308-072036-346582-55669beb-sub-01                                  │
│ [completed] [consensus_reached]                                            │
└─────────────────────────────────────────────────────────────────────────────┘
```

**Elements:**
- Requirement ID
- Status badges (completed, in_progress, pending, consensus_reached)
- Navigation back to project

### Parent Requirement Section

Shows the parent requirement context:
- Parent requirement ID
- DECOMPOSED tag
- Complexity Score (e.g., 75%)
- Full requirement text

### Sub-Requirements List

Lists all 6 sub-requirements with their statuses:

| # | ID | Title | Status |
|---|-----|-------|--------|
| 1 | sub-01 | Database Schema and Page Model | Complete |
| 2 | sub-02 | Slug Generation and URL Resolution | In Progress (cycle 2) |
| 3 | sub-03 | Admin Backend API for Page CRUD | In Progress (cycle 1) |
| 4 | sub-04 | Admin UI - Page List and Search | Pending |
| 5 | sub-05 | Admin UI - Page Create and Edit Forms | Pending |
| 6 | sub-06 | Frontend Page Display and Routing | Pending |

---

## Consensus Status Section

```
┌─────────────────────────────────────────────────────────────────────────────┐
│ Consensus Status                                                            │
├─────────────────────────────────────────────────────────────────────────────┤
│ Consensus: Pending                                                          │
│ Codex: Pending                     OpenCode: Approved ✓                    │
│                                                                             │
│ Review Cycle: 2 / 25                                                        │
│ Convergence Trend: insufficient_data                                        │
│ Next Action: terminal: consensus_reached                                    │
│ Outcome Recorded: 3/11/2026, 12:43:49 AM                                    │
└─────────────────────────────────────────────────────────────────────────────┘
```

**Fields:**
- **Consensus** - Overall consensus state (Pending, Reached, Failed)
- **Agent Status** - Per-agent consensus (Codex, OpenCode, Cursor, etc.)
- **Review Cycle** - Current iteration count (e.g., 2 of 25 max)
- **Convergence Trend** - Trend analysis (insufficient_data, converging, diverging)
- **Next Action** - Terminal state or next step
- **Outcome Recorded** - Timestamp of consensus achievement

---

## Workflow Status Section

```
┌─────────────────────────────────────────────────────────────────────────────┐
│ Workflow Status                                                             │
├─────────────────────────────────────────────────────────────────────────────┤
│ Review Cycle: 2 / 25                                                        │
│ Next Action: terminal: consensus_reached                                    │
│ Workflow Started: 3/11/2026, 12:29:05 AM                                    │
│ Created: 3/11/2026, 12:25:16 AM                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

**Timestamps:**
- When workflow started
- When requirement was created
- Last activity

---

## TDD Workflow Section

```
┌─────────────────────────────────────────────────────────────────────────────┐
│ TDD Workflow Not Started                                                    │
│ No TDD workflow found for this requirement.                                 │
│ [Start TDD Process]                                                         │
└─────────────────────────────────────────────────────────────────────────────┘
```

**Controls:**
- Start TDD Process button
- Status of TDD workflow
- Link to TDD checkpoints

---

## Iteration Timeline Section

The Iteration Timeline is the heart of the requirement detail page, showing the complete review history.

### Filters

| Filter | Options |
|--------|---------|
| Agent | All, Cursor, Codex, OpenCode, Claude, OpenRouter, DeepSeek |
| Status | All, Complete, In Progress, Failed |
| Consensus | All, Reached, Not Reached |

### Expandable Sections

Each iteration shows:

#### Reviews
Multiple concern entries per iteration:

```
## Concerns
### C1: [correctness] - [major]
**Description**: The plan references `PageController.php` in the root 
controllers directory, but this file does not contain the admin page logic...

### C2: [completeness] - [major]
**Description**: The implementation plan recommends creating a new static 
`rules()` method in `app/Models/Page.php` for validation...
```

**Concern Categories:**
- `[correctness]` - Logic or factual errors
- `[completeness]` - Missing implementation details
- `[security]` - Security-related issues
- `[performance]` - Performance concerns

**Severity Levels:**
- `[major]` - Significant issue requiring attention
- `[minor]` - Minor issue, nice to have fixed
- `[critical]` - Blocking issue

#### Feedback Decisions Table

| Column | Description |
|--------|-------------|
| Concern | Unique concern ID (e.g., OPENCODE_5-C1) |
| Decision | accept / reject |
| Accepted By | Agents that voted to accept |
| Rejected By | Agents that voted to reject |
| Applied By | Agent that implemented the decision |
| Implemented Plan | Link to view the updated plan |
| Reasoning | Detailed explanation of the decision |

##### Screenshot

![Feedback Decisions Table](./screenshots/08-feedback-decisions.png)

**Example Row:**
```
Concern:    OPENCODE_5-C1
Decision:   accept
Accepted:   opencode[feedback_decision_2] (github-copilot/gpt-4.1)
            cursor[feedback_decision_3] (kimi-k2.5)
Applied By: Opencode / deepseek/deepseek-reasoner
Plan:       [View Plan]
Reasoning:  The plan references `PageController.php` in the root 
            `Admin` folder, while pages resides in `app/Http/Controllers`...
```

#### Convergence Score

Visual indicator of how close the agents are to reaching consensus:
- Percentage score
- Trend arrow (improving/stable/worsening)

---

## Action Buttons

### Observability & Recovery

| Button | Action |
|--------|--------|
| **View Agent Logs** | See agent-specific logs |
| **View Daemon Logs** | See daemon processing logs |
| **View TDD Logs** | See TDD workflow logs |
| **View LLM Logs** | See LLM call logs for this requirement |

### Manual Controls

| Button | Action | State |
|--------|--------|-------|
| **Human Override** | Manually set requirement status | Always available |
| **Force Next Iteration** | Skip to next review cycle | Enabled when appropriate |
| **Reset Reviews** | Clear review history | Always available |
| **Hard Reset** | Complete reset of requirement | Always available |
| **Reset TDD Workflow** | Restart TDD process | When TDD active |
| **Start TDD Process** | Begin TDD workflow | When TDD not started |

---

## Usage Instructions

### Tracking Review Progress

1. Open the requirement detail page
2. Check **Consensus Status** for overall progress
3. Review **Review Cycle** count (X of 25)
4. Expand **Iteration Timeline** Details
5. Review each iteration's concerns and decisions

### Understanding Feedback Decisions

1. In the Iteration Timeline, locate the **Feedback Decisions** table
2. Review each concern's **Decision** (accept/reject)
3. Check **Accepted By** and **Rejected By** to see voting
4. Read **Reasoning** column for detailed explanations
5. Click **View Plan** to see the implemented changes

### Monitoring Consensus

1. Check **Consensus Status** section
2. Review per-agent status (Pending, Approved)
3. Note **Convergence Trend**
4. When terminal state reached, outcome is recorded

### Starting TDD Workflow

1. Ensure review cycles have reached consensus
2. Click **Start TDD Process** button
3. Monitor TDD workflow in [TDD Checkpoints](./03-tdd-checkpoints.md)

---

## Modals & Dialogs

### View Plan Modal

Clicking **"View Plan"** in the Feedback Decisions table opens a modal showing the complete implementation plan:

![Feedback Implemented Plan Modal](./screenshots/modal-feedback-implemented-plan.png)

**Modal Contents:**
- Concern ID (e.g., OPENCODE_5-C1)
- Plan Version (e.g., v2)
- Iteration number
- File path to the plan
- Full implementation plan with markdown formatting
- Executive summary
- Technical details

### Review Details Modal

Clicking **"View Full Concerns"** opens a modal with the complete agent review:

![Review Details Modal](./screenshots/modal-review-details.png)

**Modal Contents:**
- Agent name and model (e.g., Opencode - openrouter/qwen/qwen3-coder:exacto)
- List of all concerns with categories and severity
- Detailed descriptions
- Suggestions for fixes
- Overall approval status
- Meta information (plan version, timestamp)

---

## Workflow Integration

```mermaid
flowchart TD
    A[Projects] --> B[Requirement Detail]
    B --> C[Iteration Timeline]
    B --> D[Consensus Status]
    B --> E[TDD Workflow]

    C --> C1[Reviews]
    C --> C2[Feedback Decisions]
    C --> C3[Convergence Score]

    C2 --> C2a[Accept Votes]
    C2 --> C2b[Reject Votes]
    C2 --> C2c[Reasoning]

    D --> D1[Agent Consensus]
    D --> D2[Review Cycle Count]
    D --> D3[Convergence Trend]

    E --> E1[Start TDD]
    E --> E2[View TDD Logs]

    style B fill:#e1f5fe
    style C fill:#fff3e0
    style D fill:#e8f5e9
    style E fill:#fce4ec
```

---

## Benefits

### For Engineers
- **Deep Visibility** - See exactly what agents are concerned about
- **Decision Audit** - Understand why changes were accepted/rejected
- **Iteration Tracking** - Track progress through review cycles
- **Consensus Monitoring** - Know when requirement is ready for TDD

### For Tech Leads
- **Quality Control** - Review agent feedback before implementation
- **Process Oversight** - Monitor review cycle effectiveness
- **Intervention Points** - Human override when needed
- **Decision Transparency** - Full reasoning for all decisions

### For Project Managers
- **Progress Tracking** - Clear view of requirement completion status
- **Consensus Visibility** - Know when requirements are ready
- **Workflow Integration** - Seamless handoff to TDD process
- **Historical Audit** - Complete decision trail

---

## Best Practices

1. **Regular Check-ins** - Monitor active requirements daily
2. **Review Concerns** - Read through agent concerns for insights
3. **Understand Decisions** - Check reasoning before overriding
4. **Track Consensus** - Wait for consensus before starting TDD
5. **Use Filters** - Filter timeline by agent or status for focus

---

## Related Pages

- **[01 - Projects](./01-projects.md)** - Navigate to requirements from project view
- **[03 - TDD Checkpoints](./03-tdd-checkpoints.md)** - TDD workflow after consensus
- **[05 - Activity](./05-activity.md)** - Real-time activity for this requirement
- **[02 - LLM Logs](./02-llm-logs.md)** - LLM calls related to this requirement

---

## URL Pattern

```
/admin/projects/{project_name}/requirements/{requirement_id}
```

---

*Part of the Cloudvelous Engineering Workflow Documentation*
