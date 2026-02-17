# COCA Framework Skill for Claude Code

A Claude Code skill that helps you build perfect specs using the COCA framework — a lightweight spec format that doubles as an AI prompt.

## Installation

```bash
/plugin marketplace add everydev1618/coca-skill
/plugin install coca@everydev1618
```

## Usage

```bash
/coca                           # Start fresh, Claude will ask what to spec
/coca user authentication       # Jump straight into speccing a feature
```

The skill runs an interactive interview process:

1. You describe the feature and what systems it interacts with
2. Claude walks you through each COCA section one at a time
3. For each section: targeted questions → draft → refine → approve → next
4. Once all four sections are complete, you get a compiled spec

The final spec serves dual purposes: living documentation AND a structured prompt you can hand directly to Claude Code for implementation.

## The COCA Framework

COCA stands for **Context**, **Outcome**, **Constraints**, **Assertions**.

### C — Context
*Where are we starting from?*

- What exists today (current state)
- Who is the user (role, permissions, knowledge level)
- What system/codebase/stack is this part of
- Any relevant history

**Test:** Could a new engineer understand the landscape without follow-up questions?

### O — Outcome
*What does done look like?*

- The end state in concrete terms
- Who benefits and how
- What success feels like

**Test:** Could you demo this to a stakeholder and have them say "yes, that's what I wanted"?

**Trap:** Don't describe *how*—just describe *what*.

### C — Constraints
*What are the boundaries?*

- Tech stack requirements or limitations
- Timeline or budget
- Non-goals (what this is NOT)
- Security, compliance, performance requirements

**Test:** Does this prevent scope creep?

**Trap:** Don't over-constrain. Leave room for smart solutions.

### A — Assertions
*How do we know it works?*

Organized into five categories:
- **Happy Path** — Primary user flows working correctly
- **Edge Cases** — Boundary conditions and unusual scenarios
- **Error States** — What happens when things go wrong
- **Anti-Behaviors** — What should explicitly NOT happen
- **Integration** — Interactions with adjacent systems

**Test:** Could QA write test cases from this? Could an AI agent write automated tests from this?

## Example Output

```markdown
# PDF Export — COCA Spec

## Context
We have a React dashboard that displays real-time sales metrics. Users are sales
managers who check it daily. Currently there's no way to share the data with
executives who don't have login access. The dashboard uses Chart.js for
visualizations and fetches data from our existing REST API. No prior attempts
at export functionality have been made.

## Outcome
Sales managers can generate a PDF report of their dashboard with one click and
email it to executives. Executives see the same data visualization without
needing to log in. Success looks like a manager being able to share their
weekly numbers in under 30 seconds without leaving the dashboard.

## Constraints
- Must work with existing React/Node stack
- PDF must generate in under 5 seconds
- Must respect existing user permissions—managers only see their region
- No new third-party services that require security review
- Non-goal: Scheduled/automated reports (that's phase 2)
- Non-goal: Customizable report templates

## Assertions

### Happy Path
- When I click "Export PDF" on my dashboard, a PDF downloads within 5 seconds
- The PDF matches the current dashboard view, including any filters I've applied
- When I'm a manager for West region, I only see West region data in the PDF

### Edge Cases
- When the dashboard has no data, I see "No data to export" instead of an empty PDF
- When I have 100+ data points, the PDF still generates within 5 seconds

### Error States
- When PDF generation fails, I see an error message with a retry option
- When the API is unavailable, I see "Unable to generate report. Please try again."

### Anti-Behaviors
- Should NOT include data from regions the user doesn't have access to
- Should NOT allow export while data is still loading

### Integration
- When dashboard filters change, the export reflects the current filter state
- When user permissions are revoked mid-session, export respects the new permissions
```

## Why COCA Works

1. **It's promptable** — Hand the spec directly to Claude Code and get implementation-ready code
2. **It's testable** — Categorized assertions become test cases with zero translation
3. **It forces clarity** — The interview process surfaces ambiguity before you start coding
4. **It's self-contained** — A new engineer or AI agent can understand the full landscape without follow-up questions
5. **It's lightweight** — One page, not fifty. One conversation, not weeks of meetings.

## License

MIT
