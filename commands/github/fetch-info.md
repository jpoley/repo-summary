---
description: Fetch basic information about a GitHub repository including stats, license, and metadata.
---

# GitHub Repository Info Fetcher

Fetch comprehensive repository information using GitHub API tools.

## User Input

```text
$ARGUMENTS
```

## Instructions

### Step 1: Parse Repository URL

Extract the owner and repository name from the provided GitHub URL.

Supported URL formats:
- `https://github.com/owner/repo`
- `https://github.com/owner/repo.git`
- `github.com/owner/repo`
- `owner/repo`

```bash
# Parse URL to extract owner and repo
URL="$ARGUMENTS"
# Remove https://github.com/ prefix if present
CLEAN_URL=$(echo "$URL" | sed 's|https://github.com/||' | sed 's|github.com/||' | sed 's|\.git$||' | sed 's|/$||')
OWNER=$(echo "$CLEAN_URL" | cut -d'/' -f1)
REPO=$(echo "$CLEAN_URL" | cut -d'/' -f2)
echo "Owner: $OWNER"
echo "Repo: $REPO"
```

### Step 2: Fetch Repository Details

Use the GitHub MCP tools to fetch repository information:

1. **Get file contents** from the root to see structure:
   - Use `mcp__github__get_file_contents` with owner, repo, path=""

2. **Get recent commits** to assess activity:
   - Use `mcp__github__list_commits` with owner, repo, perPage=10

3. **Search for repository** to get detailed stats:
   - Use `mcp__github__search_repositories` with query="repo:owner/repo"

### Step 3: Extract Key Metrics

From the repository data, extract and report:

| Metric | Description |
|--------|-------------|
| **Name** | Repository full name |
| **Description** | Repository description |
| **Stars** | Stargazer count |
| **Forks** | Fork count |
| **Open Issues** | Number of open issues |
| **Language** | Primary programming language |
| **License** | License type (if specified) |
| **Created** | Repository creation date |
| **Last Updated** | Most recent update date |
| **Default Branch** | Main branch name |

### Step 4: Calculate Repository Age

Calculate how old the repository is:

```bash
# Example calculation (will be done by Claude)
CREATED_DATE="2020-01-15"
TODAY=$(date +%Y-%m-%d)
# Calculate age in years and months
```

### Step 5: Assess Activity Level

Based on commits data, determine:
- **Last commit date**: When was the most recent commit?
- **Commit frequency**: Approximate commits per month
- **Active contributors**: Number of unique authors in recent commits

### Output Format

Return a structured summary:

```markdown
## Repository: [owner/repo]

**Description**: [description]

### Stats
- Stars: [count]
- Forks: [count]
- Open Issues: [count]
- Primary Language: [language]

### Timeline
- Created: [date] ([X years, Y months ago])
- Last Updated: [date]
- Last Commit: [date]

### License
[License type or "Not specified"]

### Activity Assessment
- Commit Frequency: [High/Medium/Low]
- Last Activity: [Recent/Stale/Inactive]
```

## Error Handling

| Issue | Solution |
|-------|----------|
| Repository not found | Verify URL format and check if repo is public |
| Rate limiting | Wait and retry, or inform user of GitHub API limits |
| Private repository | Inform user that private repos require authentication |
