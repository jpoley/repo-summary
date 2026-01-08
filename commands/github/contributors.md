---
description: Fetch contributor statistics for a GitHub repository and generate a pie chart visualization.
---

# GitHub Contributors Analyzer

Analyze repository contributors and generate visualization of contribution distribution.

## User Input

```text
$ARGUMENTS
```

The input should be in the format: `owner repo` (space-separated)

## Instructions

### Step 1: Fetch Contributors Data

Use the GitHub API to get contributor statistics. Since we don't have a direct contributors MCP tool, use commits to analyze contributors:

1. **Fetch recent commits** using `mcp__github__list_commits`:
   - Get up to 100 recent commits
   - Extract author information from each commit

2. **Aggregate by author**:
   - Count commits per unique author
   - Calculate percentage contribution

### Step 2: Process Contributor Data

For each contributor, collect:
- **Username/Name**: Author identifier
- **Commit Count**: Number of commits
- **Percentage**: Contribution percentage of total

Sort contributors by commit count (descending).

### Step 3: Generate ASCII Pie Chart

Create a visual representation of contributor distribution.

**Pie Chart Format:**

```
Contributor Distribution
========================

    Top Contributors (by commits)

    ████████████████████  45% | contributor1 (450 commits)
    ██████████            25% | contributor2 (250 commits)
    ████████               20% | contributor3 (200 commits)
    ██                      5% | contributor4 (50 commits)
    █                       5% | others (50 commits)

    Total: 1000 commits from 15 contributors
```

**Bar Generation Rules:**
- Maximum bar width: 20 characters (█)
- Each █ represents 5% of contributions
- Round percentages to nearest 5%
- Group contributors with <3% as "others"

### Step 4: Calculate Contributor Metrics

Determine:
- **Total Contributors**: Unique author count
- **Bus Factor**: Number of contributors responsible for 80% of commits
- **Contribution Spread**: How evenly distributed are contributions?

**Bus Factor Calculation:**
```
Sort contributors by commits (descending)
Count how many contributors it takes to reach 80% of total commits
This number is the "bus factor"
```

**Contribution Spread Assessment:**
- **Concentrated**: Top contributor >50% of commits
- **Moderate**: Top 3 contributors >80% of commits
- **Distributed**: No single contributor >30%

### Step 5: Generate Report

Output the following:

```markdown
## Contributor Analysis

### Contribution Distribution

[ASCII Pie Chart Here]

### Key Metrics

| Metric | Value |
|--------|-------|
| Total Contributors | [count] |
| Bus Factor | [number] |
| Contribution Spread | [Concentrated/Moderate/Distributed] |

### Top Contributors

| Rank | Contributor | Commits | Percentage |
|------|-------------|---------|------------|
| 1 | [name] | [count] | [%] |
| 2 | [name] | [count] | [%] |
| 3 | [name] | [count] | [%] |
| 4 | [name] | [count] | [%] |
| 5 | [name] | [count] | [%] |

### Contributor Health Assessment

[Natural language paragraph describing:
- Whether contributions are well-distributed
- Risk assessment based on bus factor
- Recommendation for project sustainability]
```

## ASCII Pie Chart Helper

Generate bars using this pattern:

```
def generate_bar(percentage):
    full_blocks = int(percentage / 5)  # Each block = 5%
    return '█' * full_blocks

# Example:
# 45% -> █████████ (9 blocks)
# 25% -> █████ (5 blocks)
# 10% -> ██ (2 blocks)
# 3%  -> █ (1 block, minimum)
```

## Error Handling

| Issue | Solution |
|-------|----------|
| No commits found | Report as "No contribution data available" |
| Single contributor | Note as "Solo project" with appropriate messaging |
| API rate limits | Inform user and suggest waiting |
