# summarize-website

This repository packages a single agent skill that summarizes or compares website content from URLs or from a local file containing URLs and notes.

## Included Files

- SKILL.md
- examples/website-summary-input.md

## What It Does

- Summarizes one or more websites from direct URLs
- Summarizes sites listed in a local file
- Merges fetched page content with user notes
- Supports concise summaries and comparison-style outputs

## Install

After publishing this folder as its own Git repository, install it with the Skills CLI:

```bash
npx skills add <owner>/<repo>
```

To preview locally before publishing:

```bash
npx skills add . --list
```

## Example Prompt

```text
examples/website-summary-input.md を使ってサイト内容を要約して
```

## Notes

- The skill is designed for agents that support the shared agent skills format.
- If you publish this as a single-skill repository, keeping SKILL.md at the repository root is the simplest structure for skills.sh compatibility.