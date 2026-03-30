# news-brief-from-url-file

This repository packages a single agent skill that reads a local YAML file, checks 2 to 3 URLs for each topic, optionally searches for additional relevant sources, writes a short recent-news brief, and saves the result to a local file.

## Included Files

- SKILL.md
- examples/news-topics.yml

## What It Does

- Reads a YAML file shaped like codex-skills.yml
- Extracts keywords, related URLs, search hints, and optional notes
- Reviews only 2 to 3 URLs per topic, including sources found through targeted search when needed
- Produces a 3 to 4 line recent-news brief for each topic
- Writes the result to a local markdown file under a date-based output directory

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
examples/news-topics.yml を読んで、各トピックの直近ニュースを 3-4 行でまとめて日付ディレクトリ配下に出力して
```

## Notes

- The skill can start from provided URLs or perform limited targeted search when the URLs are missing or insufficient.
- It should still keep the investigation bounded to 2 to 3 inspected URLs per topic.
- The recommended output pattern is `outputs/{date}/...` using `YYYY-MM-DD`.
- If you publish this as a single-skill repository, keeping SKILL.md at the repository root is the simplest structure for skills.sh compatibility.