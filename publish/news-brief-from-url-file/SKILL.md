---
name: news-brief-from-url-file
description: Use this skill when the user wants to read a YAML file similar to codex-skills.yml, inspect 2 to 3 URLs associated with a keyword or topic, optionally search for additional relevant sources, summarize recent news in 3 to 4 lines, and write the result to a local file.
---

# News Brief From URL File

This skill helps create a short recent-news brief from a local YAML file that lists topics, keywords, related URLs, and optional search hints.

## When to Use This Skill

Use this skill when the user:

- Provides a YAML file and wants you to read topics from it
- Wants a concise recent-news summary for one or more keywords
- Already has related URLs and wants you to investigate only 2 to 3 of them
- Wants you to supplement the provided URLs with your own targeted web research when needed
- Wants the final summary written to a local file instead of only replying in chat

Do not use this skill when the user wants a large open-ended market scan across many unrelated sites. This workflow is for concise topic-based briefing, not broad competitive research.

## Input Format

The input file must be YAML and should be similar in spirit to codex-skills.yml: either a top-level array or a keyed array.

Supported shapes:

### 1. Top-level array

```yaml
- keyword: "OpenAI"
  urls:
    - "https://openai.com/index/"
    - "https://openai.com/news/"
    - "https://openai.com/index/gpt-4-5/"
- keyword: "TypeScript"
  urls:
    - "https://devblogs.microsoft.com/typescript/"
    - "https://github.com/microsoft/TypeScript/releases"
```

### 2. `topics` key with an array

```yaml
output:
  path: "outputs/{date}/news-brief.md"

topics:
  - keyword: "Anthropic"
    urls:
      - "https://www.anthropic.com/news"
      - "https://www.anthropic.com/research"
```

Optional fields per item:

- `keyword`: main topic or news keyword
- `urls`: related URLs to inspect
- `search`: optional additional search hints such as official domains, site names, or subtopics
- `output`: optional per-topic output file path
- `notes`: optional focus note such as "product launch only"

Optional top-level fields:

- `output.path`: default output file path for the whole run
- `output.format`: optional format hint such as `markdown`

Use a date-based output path by default. The recommended convention is `outputs/{date}/...` where `{date}` is replaced with the current date in `YYYY-MM-DD` format.

## Workflow

### Step 1: Confirm the Source File and Requested Scope

Identify:

1. The YAML file path to read
2. Which topic entries to process
3. The output file path if the user already specified one

Prefer the top-level `output.path` from the YAML file when present. If it contains `{date}`, replace it with the current date in `YYYY-MM-DD` format.

If neither the user nor the file specifies an output path, default to a dated local file such as `outputs/{date}/news-brief.md`.

### Step 2: Read and Parse the YAML File

Read the file and extract entries from either:

1. A top-level array
2. A `topics` array
3. A `news` array if present

Ignore empty entries.

For each entry, extract:

- Keyword
- Candidate URLs
- Optional search hints
- Optional output path
- Optional focus notes

Also extract the top-level default output path if present.

If a topic-level `output` is present and it contains `{date}`, replace it with the current date in `YYYY-MM-DD` format before writing.

If an entry has no URLs, or the URLs look insufficient or stale, you may perform targeted web discovery based on the keyword, notes, and search hints.

### Step 3: Discover or Refine Candidate Sources

Start from the provided URLs when they exist.

If they are missing, too generic, unavailable, or clearly not recent enough, do a limited web search to find better sources.

Preferred search targets:

- Official newsrooms, blogs, release notes, or press pages
- Official product announcement or changelog pages
- Reputable organization pages closely tied to the keyword

Use the keyword together with narrow terms such as:

- `news`
- `announcement`
- `release`
- `blog`
- `press`
- a product or organization name from the YAML entry

Keep the search bounded. The goal is still a short brief, not exhaustive research.

### Step 4: Select Only 2 to 3 URLs Per Topic

From each topic entry, inspect at most 3 URLs.

Selection rules:

- Prefer provided URLs when they are relevant and recent enough
- Prefer official news, blog, release, or press pages
- Prefer URLs that appear likely to contain recent updates
- Prefer sources found during targeted search only when they are better than the provided URLs
- Skip duplicate or clearly irrelevant pages
- If more than 3 URLs are present, pick the most promising 2 to 3 and state which ones were used

### Step 5: Fetch and Review the Pages

Use the webpage fetching tool on the selected URLs.

While reviewing the content:

- Look for publication dates or temporal clues
- Focus on concrete recent events such as launches, releases, partnerships, funding, or policy changes
- Ignore evergreen marketing copy unless it is the only available source
- Keep the distinction between confirmed page content and inference clear

If a page cannot be fetched, note that briefly and continue with the remaining URLs.

### Step 6: Write a 3 to 4 Line News Brief

For each topic, produce a brief that is:

- 3 to 4 lines total
- Focused on what happened recently
- Grounded in the fetched pages
- Clear about uncertainty when dates or recency are unclear

Preferred structure:

1. One line naming the topic and time context if known
2. One or two lines describing the most important recent development
3. One line on why it matters or what changed

### Step 7: Save the Result to a Local File

Write the output to the requested file.

Create the date directory first when the output path includes a dated folder such as `outputs/2026-03-30/`.

If multiple topics are processed into one file, use a compact structure like this:

```markdown
# News Brief

## OpenAI
2026-03-30 時点で確認できた範囲では、...
...

Sources:
- https://openai.com/index/
- https://openai.com/news/
```

Include for each topic:

- Topic name
- 3 to 4 line summary
- The 2 to 3 URLs actually used
- Whether each URL came from the YAML file or from additional search
- A short note when some candidate URLs were skipped or unavailable

## Output Guidelines

When answering the user after writing the file:

- Mention which YAML file was used
- Mention which output file was written
- Mention which date directory was created or used
- Mention which URLs were actually inspected
- Mention whether additional search was needed
- Note any entries skipped because URLs were missing or fetches failed

Keep the chat response short because the main deliverable is the written file.

## Example Triggers

- "news-topics.yml を読んで、各キーワードの直近ニュースを 3-4 行でまとめてファイルに出力して"
- "codex-skills.yml みたいな YAML を元に、URL を 2-3 本だけ調べてニュース要約を書いて"
- "topics.yml の OpenAI と Anthropic だけ見て、brief.md に保存して"
- "この YAML ファイルを読んで、関連 URL の最近の動きを短くまとめて出力して"
- "指定 URL を優先しつつ、不足分は自分で検索してニュース要約を書いて"

## Constraints

- Do not claim something is recent unless the page content supports that conclusion
- Do not fabricate facts; any additional URLs you find must come from actual targeted search
- Do not inspect more than 3 URLs per topic unless the user explicitly asks
- If you need to search, prefer official and tightly scoped sources over broad speculation