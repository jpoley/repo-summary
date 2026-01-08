---
description: Generate a comprehensive natural language summary of a GitHub repository including maturity rating, code quality assessment, license info, and contributor pie chart.
---

# GitHub Repository Summarizer

Generate comprehensive, natural language summaries of GitHub repositories with maturity ratings, code quality assessment, and contributor analysis.

## User Input

```text
$ARGUMENTS
```

## Output Directory

**IMPORTANT**: Always save summaries to `./repo-summaries/` subfolder in the current working directory.

```bash
mkdir -p ./repo-summaries
```

## Instructions

### Step 1: Parse Repository URL

Extract owner and repository name from the provided GitHub URL.

Supported formats:
- `https://github.com/owner/repo`
- `https://github.com/owner/repo.git`
- `github.com/owner/repo`
- `owner/repo`

```bash
URL="$ARGUMENTS"
CLEAN_URL=$(echo "$URL" | sed 's|https://github.com/||' | sed 's|github.com/||' | sed 's|\.git$||' | sed 's|/$||')
OWNER=$(echo "$CLEAN_URL" | cut -d'/' -f1)
REPO=$(echo "$CLEAN_URL" | cut -d'/' -f2)
```

### Step 2: Gather Repository Data

Collect comprehensive data from multiple sources:

#### 2a: GitHub API Data

Use the GitHub MCP tools to fetch:

1. **Repository search** (`mcp__github__search_repositories`):
   - Query: `repo:owner/repo`
   - Extracts: stars, forks, open issues, description, language, license, created_at, updated_at

2. **Recent commits** (`mcp__github__list_commits`):
   - Fetch up to 100 commits
   - Analyze: commit frequency, contributors, last activity

3. **File structure** (`mcp__github__get_file_contents`):
   - Path: "" (root)
   - Look for: README, LICENSE, package.json, setup.py, go.mod, etc.

4. **Open issues** (`mcp__github__list_issues`):
   - State: "open"
   - Check for: issue volume, response patterns

5. **Pull requests** (`mcp__github__list_pull_requests`):
   - Check for: open PRs, merge activity

#### 2b: DeepWiki Insights

Fetch AI-generated documentation from DeepWiki:

```
WebFetch URL: https://deepwiki.com/{owner}/{repo}
```

Extract from DeepWiki:
- Architecture overview
- Key components and systems
- Code organization patterns
- Technical insights

**Note**: DeepWiki may not have indexed all repositories. If unavailable, proceed with GitHub data only.

#### 2c: CodeWiki Insights

Fetch additional code analysis from Google's CodeWiki:

```
WebFetch URL: https://codewiki.google/github.com/{owner}/{repo}
```

Extract from CodeWiki:
- Code structure analysis
- Implementation patterns
- Technical documentation
- API insights

**Note**: CodeWiki requires the repository to have been previously scanned. If the page returns no useful content or shows an error, proceed without it and note "CodeWiki: Not indexed" in the references section.

### Step 3: Calculate Project Maturity Rating

Rate the project on a scale of 1-5 stars based on these criteria:

| Criterion | Points | Condition |
|-----------|--------|-----------|
| Age | +1 | Created > 2 years ago |
| Contributors | +1 | More than 10 unique contributors |
| Commit Volume | +1 | More than 1,000 total commits |
| Releases | +1 | Has tagged releases |
| Recent Activity | +1 | Commits within last 6 months |

**Rating Scale:**
- ★☆☆☆☆ (1): Early stage / experimental
- ★★☆☆☆ (2): Developing project
- ★★★☆☆ (3): Established project
- ★★★★☆ (4): Mature project
- ★★★★★ (5): Highly mature / production-ready

### Step 4: Assess Code Quality

Evaluate code quality based on available signals:

| Signal | Assessment |
|--------|------------|
| Documentation | README quality, inline docs, wiki presence |
| Testing | Test files present (tests/, __tests__, *_test.*, *.spec.*) |
| CI/CD | GitHub Actions, .travis.yml, Jenkinsfile presence |
| Code Organization | Clear directory structure, separation of concerns |
| Dependencies | Dependency management (package.json, requirements.txt, go.mod) |
| Security | SECURITY.md, security policy, vulnerability scanning |

Generate a qualitative assessment: **Excellent / Good / Adequate / Needs Improvement**

### Step 5: Analyze Contributors

Process contributor data:

1. Extract unique authors from commits
2. Calculate commit distribution
3. Determine bus factor
4. Generate ASCII pie chart:

```
Contributor Distribution
========================

    ████████████████████  45% | top-contributor (450 commits)
    ██████████            25% | second-contributor (250 commits)
    ████████               20% | third-contributor (200 commits)
    ██                      5% | fourth-contributor (50 commits)
    █                       5% | others (50 commits)

    Total: 1000 commits from 15 contributors
```

### Step 6: Generate Natural Language Summary

Write a comprehensive, paragraph-form summary following this structure:

```markdown
# [Repository Name] - Repository Summary

[![Repository](https://img.shields.io/github/stars/owner/repo?style=social)](https://github.com/owner/repo)

**Generated**: [Current Date]
**Source**: [GitHub URL]

---

## Overview

[2-3 paragraph natural language description of what the repository is, its purpose, and its significance in the ecosystem. Write in flowing prose, not bullet points.]

## Project Maturity

**Rating**: [★★★★☆] ([4/5])

[Natural language paragraph explaining the maturity assessment. Discuss the project's age, community size, development activity, and overall stability. Be specific about what contributes to the rating.]

## Code Quality Assessment

**Assessment**: [Excellent/Good/Adequate/Needs Improvement]

[Paragraph describing code quality signals observed. Discuss documentation, testing practices, CI/CD setup, and code organization. Provide specific examples where possible.]

## License

**Type**: [License Name] ([SPDX Identifier])

[Brief paragraph about what this license means for users. Is it permissive? Copyleft? What are the key obligations?]

## Architecture & Technical Insights

[If DeepWiki data available: Summarize key architectural patterns, main components, and technical design decisions. Write in natural language paragraphs.]

[If no DeepWiki data: Describe the repository structure based on file listing, identify main components, and note the technology stack based on files present.]

## Contributor Landscape

[ASCII PIE CHART HERE]

[Natural language paragraph analyzing the contributor distribution. Discuss the bus factor, whether contributions are well-distributed, and what this means for the project's sustainability. Highlight key contributors if notable.]

## Key Statistics

| Metric | Value |
|--------|-------|
| Stars | [count] |
| Forks | [count] |
| Open Issues | [count] |
| Contributors | [count] |
| Primary Language | [language] |
| Created | [date] |
| Last Updated | [date] |

## Quick Assessment

**Notable Features:**
- [Feature 1 observed in repository]
- [Feature 2 observed in repository]
- [Feature 3 observed in repository]

**Not Present:**
- [Standard element not found, e.g., "No test directory detected"]
- [Another missing element, if applicable]

**Best For:**
[One sentence describing the project's stated purpose based on README/description.]

---

## References

- **GitHub**: [https://github.com/owner/repo](https://github.com/owner/repo)
- **DeepWiki**: [https://deepwiki.com/owner/repo](https://deepwiki.com/owner/repo)
- **CodeWiki**: [https://codewiki.google/github.com/owner/repo](https://codewiki.google/github.com/owner/repo)
- **Documentation**: [Link if available]

---

*Summary generated by [repo-summary](https://github.com/jpoley/repo-summary) - A Claude Code Plugin*
```

### Step 7: Save Summary

Save the generated summary:

```bash
mkdir -p ./repo-summaries
FILENAME="${OWNER}-${REPO}.summary.md"
# Save content to ./repo-summaries/${FILENAME}
```

### Step 8: Confirm to User

Report completion:

1. Summary location: `./repo-summaries/[owner]-[repo].summary.md`
2. Quick preview of the Overview and Maturity Rating
3. Link to the generated file

## Guidelines for Summary Writing

1. **Natural Language**: Write in flowing paragraphs, not bullet points (except where specifically indicated)
2. **Facts Only**: Report actual numbers from the API. Do NOT editorialize or judge whether metrics are "good," "bad," "low," or "high." Let the reader interpret the data.
3. **Specific Details**: Include specific numbers, dates, and examples fetched from the API
4. **No Invented Data**: Never make up numbers. If data is unavailable, state "Not available" rather than guessing.
5. **Objective Descriptions**: Describe what exists in the repository without value judgments about adoption, popularity, or success

## Error Handling

| Issue | Solution |
|-------|----------|
| Repository not found | Verify URL and check if repo is public |
| DeepWiki unavailable | Proceed with GitHub data only, note in summary |
| CodeWiki not indexed | Proceed without CodeWiki, note "Not indexed" in references |
| Rate limiting | Inform user, suggest waiting |
| Private repository | Explain that full analysis requires public access |
