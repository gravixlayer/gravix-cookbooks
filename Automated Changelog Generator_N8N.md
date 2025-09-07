# Automated GitHub Changelog with N8N & Gravix Layer

---

## Overview

This workflow automates the generation and maintenance of a `CHANGELOG.md` file in a GitHub repository using n8n and Gravix Layer API. After every push event, it creates professional, Markdown-formatted changelog entries from Git commit data—no manual intervention required.

---

![Automated GitHub Changelog Workflow](images/n8n%20changelog%20gen%20workflow.png)

---

## Architecture & Workflow Steps

1. **Trigger**: Listens for GitHub push events.
2. **Check for Commits**: Ensures the push contains commits.
3. **Extract Commit Info**: Captures commit SHA, message, repo details, and timestamps.
4. **Get Commit Comparison**: Fetches file changes and patches from GitHub.
5. **Call Gravix Layer AI**: Sends commit data to Gravix Layer for Markdown changelog generation.
6. **Get Existing CHANGELOG.md**: Retrieves the current changelog file (if present).
7. **Merge AI & Changelog**: Combines new AI-generated entry with the existing changelog.
8. **Check Changelog Existence**: Determines if the changelog file exists.
9. **Set New Content**: Prepares the new changelog content for update or creation.
10. **Update/Create CHANGELOG.md**: Writes the new changelog to the repository.

---

## Prerequisites

- **GitHub Account**: With OAuth permissions to read/write files.
- **Gravix Layer API Key**: Obtain from [gravixlayer.com](https://gravixlayer.com/).
- **n8n Instance**: Cloud or self-hosted.

---

## Node-by-Node Breakdown

### 1. GitHub Trigger
- **Type**: `n8n-nodes-base.githubTrigger`
- **Purpose**: Listens for push events on the specified repository.
- **Config**: Uses OAuth2 credentials.

### 2. Check for Commits
- **Type**: `n8n-nodes-base.if`
- **Purpose**: Stops workflow if no commits are present in the push event.

### 3. Set Commit Info
- **Type**: `n8n-nodes-base.set`
- **Purpose**: Extracts and stores commit SHA, message, repo name/owner, before/after SHAs, and timestamp.

### 4. Get Commit Comparison

### 5. Call Gravix Layer AI
**Type**: `n8n-nodes-base.httpRequest`
**Purpose**: Sends commit message and file diffs to Gravix Layer API for AI-generated Markdown changelog.
**Endpoint**: `POST /v1/inference/chat/completions`
**Headers**: `Authorization: Bearer <API_KEY>`
- **Headers**: `Authorization: Bearer <API_KEY>`
- **Type**: `n8n-nodes-base.github`
- **Purpose**: Retrieves the current `CHANGELOG.md` file from the repository.

### 7. Merge AI and Changelog
- **Type**: `n8n-nodes-base.merge`
- **Purpose**: Merges the new AI-generated changelog entry with the existing file.

### 8. Changelog Exists?
- **Type**: `n8n-nodes-base.if`
- **Purpose**: Checks if `CHANGELOG.md` exists to decide between update or creation.

### 9. Set New Content
- **Type**: `n8n-nodes-base.set`
- **Purpose**: Formats the new changelog content for final insertion.

### 10. Update or Create CHANGELOG.md
- **Type**: `n8n-nodes-base.github`
- **Purpose**: Updates or creates the changelog file in the repository.
- **Commit Message**: Includes short commit hash for traceability.

---

## AI Prompt Example

```
You are an expert technical writer generating a single, detailed changelog entry from Git data. Your response MUST be clean Markdown and follow the rules and format below exactly. Do not add any conversational text.

RULES & FORMAT:
1. Heading: Create a level 2 Markdown heading (##) from the first line of the commit message and the date.
2. Overview Paragraph: Write a concise paragraph summarizing the overall goal of the commit.
3. Detailed Changes Section: Under ### Detailed Changes, write a bulleted list of specific code-level changes.
4. Commit Hash: Include the short commit hash.
5. File Summary Section: Include lists for 'Added', 'Modified', and 'Removed' files.

Input Data:
- Commit Message
- File Patches
```

---

## Security Best Practices

- Never hardcode API keys or OAuth tokens in workflow JSON.
- Use n8n credential management for secrets.
- Restrict repository access and rotate API keys regularly.

---

## Example GitHub API Calls

- Compare commits: `GET /repos/{owner}/{repo}/compare/{before_sha}...{after_sha}`
- Retrieve file: `GET /repos/{owner}/{repo}/contents/CHANGELOG.md`
- Create/update file: `PUT /repos/{owner}/{repo}/contents/CHANGELOG.md`

---

## Full Workflow JSON

```json
{
  "nodes": [
    {
      "parameters": {
        "authentication": "oAuth2",
        "owner": "<GITHUB_USERNAME>",
        "repository": "<REPOSITORY_NAME>",
        "events": ["push"],
        "options": {}
      },
      "id": "<ID>",
      "name": "GitHub Trigger",
      "type": "n8n-nodes-base.githubTrigger",
      "typeVersion": 1,
      "position": [-1104, 64],
      "webhookId": "<WEBHOOK_ID>",
      "credentials": {"githubOAuth2Api": {"id": "<CREDENTIAL_ID>", "name": "GitHub account"}}
    },
    {
      "parameters": {
        "values": {
          "string": [
            {"name": "commit_sha", "value": "={{ $json.body.head_commit.id }}"},
            {"name": "commit_message", "value": "={{ $json.body.head_commit.message }}"},
            {"name": "repository_name", "value": "={{ $json.body.repository.name }}"},
            {"name": "repository_owner", "value": "={{ $json.body.repository.owner.login }}"},
            {"name": "before_sha", "value": "={{ $json.body.before }}"},
            {"name": "after_sha", "value": "={{ $json.body.after }}"},
            {"name": "commit_timestamp", "value": "={{ $json.body.head_commit.timestamp }}"}
          ]
        },
        "options": {}
      },
      "id": "<ID>",
      "name": "Set Commit Info",
      "type": "n8n-nodes-base.set",
      "typeVersion": 1,
      "position": [-672, 64]
    },
    {
      "parameters": {
        "authentication": "oAuth2",
        "resource": "file",
        "operation": "get",
        "owner": {"__rl": true, "value": "={{ $(\"Set Commit Info\").item.json.repository_owner }}", "mode": ""},
        "repository": {"__rl": true, "value": "={{ $(\"Set Commit Info\").item.json.repository_name }}", "mode": ""},
        "filePath": "CHANGELOG.md",
        "asBinaryProperty": false,
        "additionalParameters": {}
      },
      "id": "<ID>",
      "name": "Get Existing CHANGELOG",
      "type": "n8n-nodes-base.github",
      "typeVersion": 1,
      "position": [-448, 176],
      "webhookId": "<WEBHOOK_ID>",
      "credentials": {"githubOAuth2Api": {"id": "<CREDENTIAL_ID>", "name": "GitHub account"}}
    },
    {
      "parameters": {"mode": "combine", "combinationMode": "mergeByPosition", "options": {}},
      "id": "<ID>",
      "name": "Merge AI and Changelog",
      "type": "n8n-nodes-base.merge",
      "typeVersion": 2.1,
      "position": [-16, 64]
    },
    {
      "parameters": {
        "authentication": "oAuth2",
        "resource": "file",
        "operation": "edit",
        "owner": "={{ $node[\"Set Commit Info\"].json[\"repository_owner\"] }}",
        "repository": "={{ $node[\"Set Commit Info\"].json[\"repository_name\"] }}",
        "filePath": "CHANGELOG.md",
        "fileContent": "={{ $(\"Set New Content\").item.json.choices[0].message.content }}",
        "commitMessage": "=docs: Update CHANGELOG.md from commit {{ $(\"Set Commit Info\").item.json.commit_sha.substring(0, 7) }}"
      },
      "id": "<ID>",
      "name": "Update CHANGELOG.md",
      "type": "n8n-nodes-base.github",
      "typeVersion": 1,
      "position": [656, -64],
      "webhookId": "<WEBHOOK_ID>",
      "credentials": {"githubOAuth2Api": {"id": "<CREDENTIAL_ID>", "name": "GitHub account"}}
    },
    {
      "parameters": {
        "conditions": {"string": [{"value1": "={{ $json.body.commits }}", "operation": "isNotEmpty"}]}
      },
      "id": "<ID>",
      "name": "Check for Commits",
      "type": "n8n-nodes-base.if",
      "typeVersion": 1,
      "position": [-896, 64]
    },
    {
      "parameters": {
        "conditions": {"string": [{"value1": "={{ $input.item.json.sha }}", "operation": "isNotEmpty"}]}
      },
      "id": "<ID>",
      "name": "Changelog Exists?",
      "type": "n8n-nodes-base.if",
      "typeVersion": 1,
      "position": [224, 64]
    },
    {
      "parameters": {
        "authentication": "oAuth2",
        "resource": "file",
        "owner": "={{ $node[\"Set Commit Info\"].json[\"repository_owner\"] }}",
        "repository": "={{ $node[\"Set Commit Info\"].json[\"repository_name\"] }}",
        "filePath": "CHANGELOG.md",
        "fileContent": "={{ $input.item.json.choices.message.content }}",
        "commitMessage": "=docs: Create CHANGELOG.md from commit {{ $(\"Set Commit Info\").item.json.commit_sha.substring(0, 7) }}"
      },
      "id": "<ID>",
      "name": "Create CHANGELOG.md",
      "type": "n8n-nodes-base.github",
      "typeVersion": 1,
      "position": [432, 176],
      "webhookId": "<WEBHOOK_ID>",
      "credentials": {"githubOAuth2Api": {"id": "<CREDENTIAL_ID>", "name": "GitHub account"}}
    },
    {
      "parameters": {
        "values": {
          "string": [{"name": "new_content", "value": "={{ ($json.choices && $json.choices[0]?.message?.content ? $json.choices[0].message.content : '') + '\n\n---\n\n' + ($json.content ? Buffer.from($json.content, 'base64').toString('utf8') : '') }}"}]
        },
        "options": {}
      },
      "id": "<ID>",
      "name": "Set New Content",
      "type": "n8n-nodes-base.set",
      "typeVersion": 1,
      "position": [432, -64]
    },
    {
      "parameters": {
        "url": "=https://api.github.com/repos/{{ $node[\"Set Commit Info\"].json.repository_owner }}/{{ $node[\"Set Commit Info\"].json.repository_name }}/compare/{{ $node[\"Set Commit Info\"].json.before_sha }}...{{ $node[\"Set Commit Info\"].json.after_sha }}",
        "sendHeaders": true,
        "headerParameters": {"parameters": [{"name": "Accept", "value": "application/vnd.github.v3+json"}]},
        "options": {}
      },
      "id": "<ID>",
      "name": "Get Commit Comparison",
      "type": "n8n-nodes-base.httpRequest",
      "typeVersion": 4.2,
      "position": [-448, -64]
    },
    {
      "parameters": {
        "method": "POST",
        "url": "https://api.gravixlayer.com/v1/inference/chat/completions",
        "sendHeaders": true,
        "headerParameters": {"parameters": [{"name": "Authorization", "value": "=Bearer <GRAVIXLAYER_API_KEY>"}]},
        "sendBody": true,
        "specifyBody": "json",
        "jsonBody": "={{ { 'model': 'meta-llama/llama-3.1-8b-instruct', 'messages': [{ 'role': 'user', 'content': 'Your formatted AI prompt here.' }] } }}",
        "options": {}
      },
      "id": "<ID>",
      "name": "Call Gravix Layer AI",
      "type": "n8n-nodes-base.httpRequest",
      "typeVersion": 4.2,
      "position": [-224, -64]
    }
  ],
  "connections": { ... },
  "pinData": {},
  "meta": {"instanceId": "<INSTANCE_ID>"}
}
```

---

## Conclusion

This workflow enables:
- Fully automated changelog generation and management.
- AI-generated documentation from Git commit history.
- Seamless, scalable, and professional changelog maintenance.

The system is modular and extensible, allowing future integrations (e.g., Slack notifications, semantic versioning, alternative LLMs).

---

## Next Steps

- Deploy on production repositories.
- Monitor execution for first few runs.
- Refine prompt or AI provider as needed.
- Add notification systems if required.

For enhancements, fork and modify your workflow using n8n's visual interface or JSON editor.
