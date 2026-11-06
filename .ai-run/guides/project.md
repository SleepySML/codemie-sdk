# Project Context

## Project Identity

| Field | Value | Source |
|---|---|---|
| Project name | codemie-sdk | sdk/codemie-nodejs/package.json::name |
| Repository/package | codemie-sdk | sdk/codemie-nodejs/package.json::repository.url |
| Project code/key | EPMCDME | user decision |

## Work Item Tracker

| Field | Value |
|---|---|
| Provider | Jira |
| Key/prefix | EPMCDME |

## Ticket Adapter

**Status**: configured
**Adapter**: Invoke the `codemie-jira-assistant` skill via the Skill tool.
**Lookup**: Invoke the `codemie-jira-assistant` skill with the ticket key and a request for summary, description, acceptance criteria, and links.
**Create**: Invoke the `codemie-jira-assistant` skill with the complete ticket payload or approved story file as the argument.
**Output**: Ticket key and URL returned by the skill.
**Comment**: Invoke the `codemie-jira-assistant` skill with a request to add a comment to the ticket, passing the comment body as the argument.

## Source Control And Review

| Field | Value |
|---|---|
| Provider | GitHub |
| Repository remote | https://github.com/codemie-ai/codemie-sdk.git |
| Default target branch | main |
| Review artifact type | PR |

## MR Adapter

**Status**: not configured
**Adapter**: not configured
**Instructions**: not configured

## Complexity Scoring

**Status**: configured
**Field**: Labels
**Format**: t-shirt
