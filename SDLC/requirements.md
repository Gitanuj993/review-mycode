# Phase 1 : Requirement gathering 

project name : review-mycode
descriptions : It is an AI code reviewer which review pull requests
and provide valuable suggestions, checks for vulnerability and ensure secuirty.


## Problem Statement 

Manual  pull requests reviews by solo developers and small team   are time-consuming, inconsistent and can have many vulnerabilities.
Small teams and solo developers often receive delayed or no feedback, allowing bugs, security issues, and poor coding practices.

We observed a need of a system which automatically review pull requests, comment on PR, Comment on Comments, provide code suggestion.


## Expected Features

1. Automatic PR Review.
2. Comment on PR.
3. Fix | Quick Win issues

## Stakeholders

1. AI Code reviewer @review-mycode
2. LLM API 
3. Github Actions
4. Devloper who Trigger code reviewer


## Functional Requirements

### FR-1: Trigger Reviews
The system shall start a review when a repository_dispatch event is received.


### FR-2: Receive Repository Information
The system shall receive:
Repository name
Pull Request number
Commit SHA (optional but recommended)
Branch (optional)


### FR-3: Retrieve Changed Files
The system shall fetch the list of files modified in the pull request.


### FR-4: Analyze Code
The system shall send the changed code to an LLM for analysis.

### FR-5: Generate Review
The system shall generate feedback including:
Bugs
Suggestions
Security issues
Performance concerns
Readability improvements
Best practice violations

### FR-6: Post Review
The system shall publish the review as a GitHub PR comment.


### FR-7: Handle Errors *
The system shall gracefully handle:
API failures
Invalid repository
Missing permissions
Rate limits
LLM failures

### FR-8: Log Execution
The system shall log each major step for debugging and auditing.


## Non-Functional Requirements (NFR)

### Performance
Complete review within approximately 2 minutes for typical pull requests.

### Reliability
Failures should not crash the workflow.
Clear error messages should be produced.
### Security
API keys must never be exposed.
GitHub tokens must use least-privilege permissions.
Secrets must be stored in GitHub Secrets.

### Scalability
The reviewer should support multiple repositories without code changes.
Maintainability
Modular architecture.
Independent components.
Easy to replace the LLM provider.

### Portability
Should run on GitHub-hosted runners without custom infrastructure.


### Assumptions *
The target repository has an open pull request.
The reviewer has permission to read the repository.
The LLM API is available.
GitHub Actions are enabled.

### Constraints
Reviews should consider only changed files in the pull request.
Token limits of the LLM must be respected.
The workflow is triggered only via repository_dispatch in the MVP.*


# System Inputs and Outputs

## Inputs

| ID | Input | Description | Source | Required |
|----|-------|-------------|--------|----------|
| IN-01 | Repository Name | Full name of the target repository (e.g., `owner/repository`) | `repository_dispatch` payload | ✅ |
| IN-02 | Pull Request Number | Pull request to be reviewed | `repository_dispatch` payload | ✅ |
| IN-03 | Commit SHA | Commit hash associated with the review request | `repository_dispatch` payload | ✅ |
| IN-04 | GitHub Token | Authentication token for accessing GitHub APIs | GitHub Secrets | ✅ |
| IN-05 | LLM API Key | API key for communicating with the LLM provider | GitHub Secrets | ✅ |
| IN-06 | Review Prompt | System prompt and review instructions for the LLM | Prompt Template (`.md`/`.txt`) | ✅ |
| IN-07 | LLM Model | Name of the model used for code review (e.g., `gpt-5`, `claude`, `gemini`) | Configuration | ✅ |
| IN-08 | Repository Diff | Changed files and code modifications in the pull request | GitHub REST API | Generated |
| IN-09 | Configuration | Reviewer settings (ignored files, severity, max tokens, etc.) | Config File | Optional |

---

## Outputs

| ID | Output | Description | Destination |
|----|--------|-------------|-------------|
| OUT-01 | Pull Request Review | AI-generated review comment summarizing findings | GitHub Pull Request |
| OUT-02 | Review Suggestions | Actionable recommendations for improving code quality | GitHub Pull Request |
| OUT-03 | Security Findings | Potential vulnerabilities detected in the code | GitHub Pull Request |
| OUT-04 | Performance Suggestions | Recommendations to improve efficiency and performance | GitHub Pull Request |
| OUT-05 | Code Quality Report | Readability, maintainability, and best-practice observations | GitHub Pull Request |
| OUT-06 | Workflow Logs | Execution logs for debugging and auditing | GitHub Actions Logs |
| OUT-07 | Error Report | Details of failures such as API errors or permission issues | GitHub Actions Logs |
| OUT-08 | LLM Response | Raw or parsed response from the LLM before formatting | Internal Processing |
| OUT-09 | Execution Status | Final workflow result (Success/Failure) | GitHub Actions |


## Visions :  Out of Scope (MVP)

To keep the first release focused, we explicitly exclude:
Inline review comments on specific lines.
Automatic code fixes.
Multi-model routing.
Local/offline LLM support.
Persistent database or review history.
Repository-specific coding rules.
Web dashboard or analytics.
Human approval workflows.
The system does not automatically modify or merge code.


#  Phase 1: Requirement Gathering Checklist

- [x] Project Title
- [x] Problem Statement
- [x] Project Goal / Objectives
- [x] Project Scope (In Scope / Out of Scope)
- [x] Stakeholders
- [x] Functional Requirements (FR)
- [x] Non-Functional Requirements (NFR)
- [x] System Inputs
- [x] System Outputs
- [x] Assumptions
- [x] Constraints
- [x] High-Level Use Case
- [x] Success Criteria *(Optional but Recommended)*
- [] Glossary *(Optional but Recommended)*
