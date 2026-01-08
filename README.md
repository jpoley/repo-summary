# repo-summary

A Claude Code plugin that generates comprehensive, natural language summaries of GitHub repositories.

Stop sifting through READMEs, stars, and commit histories. Get an instant, AI-powered assessment of any public GitHub repository—complete with maturity ratings, code quality analysis, and contributor insights.

## Features

| Feature | Description |
|---------|-------------|
| **Project Maturity Rating** | 1-5 star assessment based on age, community size, commit volume, releases, and recent activity |
| **Code Quality Assessment** | Evaluation of documentation, testing, CI/CD, and code organization |
| **License Analysis** | Plain-English explanation of what the license means for you |
| **Contributor Pie Chart** | ASCII visualization showing contribution distribution and bus factor |
| **Architecture Insights** | Technical overview pulled from DeepWiki and Google CodeWiki |
| **Natural Language Output** | Flowing prose summaries, not just bullet points |

## Installation

```bash
claude plugins add jpoley/repo-summary
```

## Usage

Generate a summary for any public GitHub repository:

```bash
/repo-summarize https://github.com/facebook/react
```

Accepts multiple URL formats:

```bash
/repo-summarize facebook/react
/repo-summarize github.com/facebook/react
/repo-summarize https://github.com/facebook/react.git
```

### Sub-commands

Fetch basic repository info only:
```bash
/github:fetch-info https://github.com/owner/repo
```

Analyze contributors only:
```bash
/github:contributors owner repo
```

## Example Output

Summaries are saved to `./repo-summaries/[owner]-[repo].summary.md`:

```markdown
# React - Repository Summary

## Overview

React is a declarative, efficient, and flexible JavaScript library for building
user interfaces. Originally developed at Facebook and released as open source
in 2013, it has become one of the most influential frontend frameworks in the
modern web development ecosystem...

## Project Maturity

**Rating**: ★★★★★ (5/5)

React exemplifies a highly mature open-source project. With over a decade of
active development, more than 1,600 contributors, and consistent release
cycles, it demonstrates exceptional stability and community investment...

## Contributor Landscape

Contributor Distribution
========================

    ████████████████████  12% | gaearon (1,847 commits)
    ███████████████       9%  | acdlite (1,402 commits)
    ██████████████        8%  | sebmarkbage (1,243 commits)
    ████████              5%  | sophiebits (812 commits)
    ██████████████████   44%  | others (6,892 commits)

    Total: 12,196 commits from 1,647 contributors

The contribution distribution suggests a healthy project with strong
core maintainers and broad community participation...
```

## How Maturity Ratings Work

Projects are rated 1-5 stars based on objective criteria:

| Criterion | +1 Star If... |
|-----------|---------------|
| **Age** | Created more than 2 years ago |
| **Contributors** | Has 10+ unique contributors |
| **Commits** | More than 1,000 total commits |
| **Releases** | Has tagged releases |
| **Activity** | Commits within the last 6 months |

| Rating | Meaning |
|--------|---------|
| ★☆☆☆☆ | Early stage / experimental |
| ★★☆☆☆ | Developing project |
| ★★★☆☆ | Established project |
| ★★★★☆ | Mature project |
| ★★★★★ | Highly mature / production-ready |

## Data Sources

The plugin aggregates data from multiple sources:

- **GitHub API** — Repository stats, commits, contributors, issues, pull requests
- **[DeepWiki](https://deepwiki.com)** — AI-generated architecture documentation
- **[Google CodeWiki](https://codewiki.google)** — Code analysis and implementation patterns (when indexed)

## Output Sections

Each summary includes:

1. **Overview** — What the project is and why it matters
2. **Project Maturity** — Star rating with detailed justification
3. **Code Quality** — Assessment of engineering practices
4. **License** — Type and practical implications
5. **Architecture** — Technical design and patterns
6. **Contributors** — Distribution chart and sustainability analysis
7. **Statistics** — Key metrics at a glance
8. **Quick Assessment** — Strengths, improvements, and who it's best for

## Requirements

- [Claude Code](https://claude.ai/code) CLI
- GitHub MCP tools enabled (included with Claude Code)

