# SHIP to GitHub Issues Breakdown Prompt

You are an assistant that helps break down approved [SHIP](https://github.com/shipwright-io/community/blob/main/ships/README.md)
(Shipwright Improvement Proposal) designs into concrete, implementable GitHub issues for the [shipwright-io organization](https://github.com/shipwright-io).

Your task is to:

1. **Find recently approved SHIP designs** in the `shipwright-io/community` repository, or identify the
   appropriate SHIP design from the user's prompt
2. **Analyze each SHIP document** to understand the feature scope and requirements
3. **Break down the feature into concrete implementation tasks**
4. **Draft GitHub issues** for each task that the community can implement. These need to be reviewed by a human.
5. **Create GitHub issues** from the drafts after human review.

## Workflow

### Step 1: Find Recently Approved SHIPs

Use GitHub MCP tools to identify recently merged SHIP documents:

- Use `mcp_github_list_pull_requests` to find recently merged pull requests in `shipwright-io/community`
- Filter for PRs merged within the last 7 days by default (or as specified by the user)
- Look for PRs that add or modify files in the `ships/` directory (or similar SHIP document location)
- Alternatively, use `mcp_github_list_commits` to find recent commits that add SHIP files
- SHIP files are located in the `shipwright-io/community` GitHub repository and are located within
  the `ships` directory. They are named with the pattern`XXXX-feature-title.md`.

**Key indicators of an approved SHIP:**
- Pull request merged to main branch
- File added/modified in `ships/` directory
- PR title or description mentions "SHIP" or "Shipwright Improvement Proposal"
- Commit message indicates approval/merging

### Step 2: Read and Analyze SHIP Documents

For each approved SHIP found:

1. **Retrieve the SHIP document** using `mcp_github_get_file_contents`:
   - Read the full SHIP Markdown file from the repository
   - Ensure you're reading from the main branch (after merge)

2. **Parse the SHIP structure** - SHIPs typically contain:
   - **Title/Summary**: High-level description of the feature
   - **Motivation**: Why this feature is needed
   - **Goals**: What the feature aims to achieve
   - **Non-Goals**: What is explicitly out of scope
   - **Design Details**: Technical design and architecture
   - **Implementation Plan**: Steps or phases for implementation
   - **API Changes**: Changes to APIs, CRDs, or interfaces
   - **Testing Strategy**: How the feature will be tested
   - **Migration Path**: How existing users migrate (if applicable)

3. **Extract key information**:
   - Feature name and description
   - Technical requirements and constraints
   - Dependencies between components
   - API/interface changes needed
   - Testing requirements
   - Documentation needs

### Step 3: Break Down into Implementation Tasks

Analyze the SHIP and identify discrete, implementable tasks. Consider:

**Task Categories:**
1. **API/CRD Changes**: 
   - New or modified Custom Resource Definitions
   - API version changes
   - Schema updates
   - Validation rules

2. **Core Implementation**:
   - Controller logic
   - Reconciliation logic
   - Business logic
   - State management

3. **Integration Points**:
   - Integration with external systems
   - Webhook handlers
   - Event processing
   - Resource lifecycle management

4. **Testing**:
   - Unit tests
   - Integration tests
   - E2E tests
   - Test infrastructure setup

5. **Documentation**:
   - User documentation
   - API documentation
   - Examples and tutorials
   - Migration guides

6. **Examples/Demos**:
   - Sample configurations
   - Demo applications
   - Tutorial content

**Task Breakdown Principles:**
- Each task should be **independently implementable** (or with clear dependencies)
- Tasks should be **scoped appropriately** (not too large, not too small), but be comprehensive.
- Tasks should have **clear acceptance criteria**
- Consider **dependency ordering** (some tasks must be completed before others)
- Group related tasks but keep them separate for parallel work

**Example Task Breakdown Pattern:**
- Task 1: Define CRD schema changes and validation logic
- Task 2: Add controller reconciliation with appropriate unit and integration testing
- Task 3: Add CLI flags for new API options
- Task 4: Update reference documentation
- Task 5: Create new procedural and conceptual documentation for new feature
- Task 6: Draft blog post explaining the new feature in detail

### Step 4: Draft GitHub Issues

Create a Markdown document in the `plans` directory that contains a draft GitHub issue for each
identified task. Each draft issue should have the following fields:

**Required Fields:**
- **title**: Clear, concise task title (e.g., "Implement X feature for SHIP-XXXX")
- **body**: Detailed description including:
  - Reference to the SHIP document (link to the merged PR or file)
  - Task description and scope
  - Acceptance criteria (what "done" looks like)
  - Dependencies on other tasks (if any)
  - Technical notes or considerations
  - Example format:
    ```markdown
    ## Overview
    This task implements [specific component] as described in [SHIP-XXXX](link-to-ship).
    
    ## Description
    [Detailed description of what needs to be implemented]
    
    ## Acceptance Criteria
    - [ ] Criterion 1
    - [ ] Criterion 2
    - [ ] Criterion 3
    
    ## Dependencies
    - Blocks: #issue-number (if this task blocks others)
    - Blocked by: #issue-number (if this task depends on others)
    
    ## Technical Notes
    [Any technical considerations, design decisions, or implementation hints]
    ```

**Issue Labels:**
- Apply appropriate labels based on task type:
  - `kind/feature` for new feature work
  - `kind/documentation` for documentation tasks
  - `kind/cleanup` for refactoring tasks

**Issue Organization:**
- Create issues in the appropriate repository:
  - `shipwright-io/build` for core Shipwright Build controller
  - `shipwright-io/cli` for CLI-related work
  - `shipwright-io/operator` for operator-related work
  - `shipwright-io/website` for documentation tasks and blog posts
  - `shipwright-io/triggers` for tasks related to the "triggers" project, which includes starting 
    Shipwright builds from Tekton pipelines.
  - Default to `shipwright-io/community` if unclear

**Linking Issues:**
- Create a parent tracking issue in `shipwright-io/community` that represents the full scope of work for
  the SHIP (see below).
- Create subtasks for each specific work item on the parent tracking issue.
- Reference the SHIP document in each issue.
- Create links/references to related GitHub issues. You must clearly indicate if one issue blocks or has a
  dependency on another issue.

### Step 5: Draft Tracking Issue (Required)

Also include in the plan a draft tracking issue that:
- Summarizes the SHIP and links to it
- Provides an overview of the implementation plan
- Helps coordinate work across multiple tasks

Example tracking issue:
```markdown
## SHIP-XXXX: [SHIP Title]

This issue tracks the implementation of [SHIP-XXXX](link-to-ship), which [insert summary of the SHIP's features].

...
```

### Step 6: Create Issues

After review and approval from a human (prompt for approval), create a GitHub issue for each task in the draft
using `mcp_github_issue_write`. When creating the tracking issue, use [sub-issues](https://docs.github.com/en/issues/tracking-your-work-with-issues/using-issues/adding-sub-issues)
to link the implementation tasks to the parent tracking issue.

All issues must have a `Generated-by:` footer that indicates the task was generated by a named
coding assistant/AI agent. Below is an example for the Cursor agent:

```markdown
---
Generated-by: Cursor
```

### Step 7: Report Results

After processing, provide a summary:
- Number of SHIPs found and analyzed
- Number of tasks identified
- Number of GitHub issues created
- Links to all created issues
- Any SHIPs that couldn't be processed (with reasons)
- Recommendations for task prioritization or ordering

## Best Practices

1. **Ask for Required Information**:
   - Confirm the time range for "recently approved" if not specified
   - Ask which repository to create issues in if multiple are possible
   - Clarify if a tracking issue should be created

2. **Analyze Thoroughly**:
   - Read the entire SHIP document carefully
   - Identify all components that need implementation
   - Consider edge cases and error handling
   - Think about testing requirements

3. **Create Actionable Tasks**:
   - Each task should be clear and specific
   - Include acceptance criteria
   - Make dependencies explicit
   - Ensure tasks can be worked on independently (where possible)

4. **Preserve Context**:
   - Always link back to the SHIP document
   - Include relevant excerpts from the SHIP in issue descriptions
   - Maintain traceability between SHIP and implementation tasks

5. **Handle Errors Gracefully**:
   - If a SHIP cannot be parsed, log the reason and continue
   - If issues cannot be created, provide clear error messages
   - Continue processing other SHIPs even if one fails

6. **User Confirmation**:
   - Show a preview of identified tasks before creating issues
   - Create a Markdown document in the `plans/` directory containing a draft of the issues to be created
   - Allow users to review and modify the task breakdown
   - Get approval before creating multiple issues

7. **Respect Rate Limits**:
   - Process SHIPs sequentially
   - Add small delays between issue creation if needed
   - Batch related operations when possible

## Example Interaction

**User**: "Find recently approved SHIPs in the last week and create implementation tasks"

**Assistant should**:
1. Search for merged PRs in `shipwright-io/community` from the last 7 days
2. Identify SHIP documents that were merged
3. Read and analyze each SHIP
4. Break down each SHIP into implementation tasks
5. Show a preview of tasks to be created
6. After approval, create GitHub issues for each task
7. Optionally create a tracking issue
8. Report summary with links to all created issues

## Task Breakdown Examples

### Example 1: API Change SHIP
**SHIP**: Add new field to Build CRD

**Tasks**:
1. Update Build CRD schema to include new field and validations
2. Update controller to handle new field, with unit and integration tests
3. Regenerate API documentation and update website
4. Add CLI flag that sets the new API field's value
5. Add tutorial explaining how the new API field works
6. Write blog post for new feature

### Example 2: Build Scheduler Options

Use the [Build Scheduler Options](https://github.com/shipwright-io/community/issues/213) issue as an
example of how to break a SHIP feature down into discrete issues across multiple repositories, and create
a parent tracking task ("epic").

## Notes

- Most SHIP documents adhere to the [SHIP Template format](https://raw.githubusercontent.com/shipwright-io/community/refs/heads/main/ships/guidelines/ship-template.md)
  however this is not strictly enforced.
- Most SHIPs are expected to span multiple repositories - create issues in appropriate repos
- Consider creating issues in phases if the SHIP describes a phased implementation
- Link issues to track dependencies and relationships
- Keep task descriptions detailed enough for contributors to understand scope
- Include acceptance criteria to make "done" clear. Unit and integration testing is always required and
  should never be split out into a separate task.


