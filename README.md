# Builds for OpenShift - AI Agent Prompts

This repository contains AI agent prompts used for managing the workflow of breaking down Shipwright features into GitHub issues and syncing them to Red Hat's JIRA instance.

## Purpose

This repository provides structured prompts that enable AI agents to:

1. **Sync GitHub issues to JIRA**: Migrate recently created GitHub issues from the `shipwright-io` organization to the Builds for OpenShift (`BUILD`) project on [Red Hat's JIRA instance](https://issues.redhat.com).

## Contents

### Prompts

- **`prompts/GITHUB-TO-JIRA.md`**: Prompt for syncing GitHub issues to Red Hat's JIRA
  - Searches for recently created GitHub issues in the `shipwright-io` organization
  - Extracts issue information and maps GitHub metadata to JIRA fields
  - Creates corresponding JIRA issues in the `BUILD` project
  - Establishes bidirectional links between GitHub and JIRA issues

## Usage

These prompts are designed to be used with AI agents that have access to:

- **GitHub MCP tools**: For interacting with GitHub repositories, issues, and pull requests
- **JIRA MCP tools**: For creating and managing issues in Red Hat's JIRA instance

### Prerequisites

- Create a GitHub Personal Access Token that grants permission to create GitHub issues on the `shipwright-io`
  organization (allowed by default).
- Create a Personal Access Token for [Red Hat JIRA](`https://issues.redhat.com`)
- Permissions to create issues in the `BUILD` JIRA project (allowed by default)
- MCP servers configured for [GitHub](https://github.com/github/github-mcp-server/tree/main) and
  [JIRA](https://github.com/sooperset/mcp-atlassian) for your coding assistant of choice.

### Example Usage

**Syncing to JIRA:**
```
Use the prompt in prompts/GITHUB-TO-JIRA.md to migrate recently created 
GitHub issues to the BUILD project in Red Hat's JIRA.
```

## Key Mappings

### GitHub to JIRA Issue Types
- `kind/bug` → `Bug`
- `kind/feature` → `Story`
- `kind/cleanup` → `Task`
- Default → `Story`

### GitHub to JIRA Milestones
- `release-v0.19` → `builds-1.8`
- `release-v0.20` → `builds-1.9`
- `release-v0.21` → `builds-1.10`

### Repository Mapping
GitHub issues are created in appropriate repositories:
- `shipwright-io/build` - Core Build controller
- `shipwright-io/cli` - CLI-related work
- `shipwright-io/operator` - Operator-related work
- `shipwright-io/website` - Documentation and blog posts
- `shipwright-io/triggers` - Tekton pipeline integration
- `shipwright-io/community` - Tracking issues and community work

## Notes

- GitHub uses Markdown formatting, while Red Hat's JIRA instance does not support Markdown in descriptions. The prompts include guidance for handling this conversion.
- User mappings between GitHub and JIRA may require manual verification.
- Custom fields in JIRA may need special handling based on project configuration.
- The prompts include error handling, rate limiting considerations, and best practices for bulk operations.

## Contributing

When updating these prompts:
- Ensure workflow steps are clear and actionable
- Include error handling guidance
- Document any new mappings or configurations
- Test with actual SHIPs and GitHub issues before committing changes

## License

```
Copyright 2025 Adam B Kaplan

SPDX-License-Identifier: Apache-2.0
```
