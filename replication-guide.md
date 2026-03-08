# Replication guide: government program research with Firecrawl + Claude Code

A nonprofit preparing for a government meeting needs to know: which programs align with ours, what terminology to use, and how to formally partner. We automated the research phase using a web crawler and an AI coding assistant. The whole process took about an hour.

## Step 1: crawl with Firecrawl

We used [Firecrawl](https://www.firecrawl.dev/) to crawl `icbf.gov.co` from the homepage—100 pages, output as markdown (one `.md` file per page). This captured the main program descriptions and org structure but missed deeper operational manuals. A URL-pattern filter or higher page limit would improve coverage.

## Step 2: analyze with Claude Code

We opened Claude Code in the crawled-files directory and used [plan mode](https://docs.anthropic.com/en/docs/claude-code/how-to#use-plan-mode-for-complex-tasks) to explore without modifying files:

1. **Orientation:** "ACTRA does group CBT for crime prevention with adolescents in Bogotá. Help me find where it fits within ICBF's structure."
2. **Exploration:** keyword searches across the crawled files (prevención del delito, organizaciones aliadas, responsabilidad penal, etc.) and reading ~10 key pages.
3. **Synthesis:** identified three entry points, the EPRE contracting mechanism, and terminology mappings.
4. **Report generation:** exited plan mode and wrote the full Spanish report with direct quotes and ready-to-use phrases.

## Step 3: publish

```bash
git init && git add . && git commit -m "ACTRA-ICBF strategic alignment report"
gh repo create actra-icbf-alignment --public --source=. --push
```

## Tips

- **Crawl more, filter aggressively.** Government sites are 30–50% boilerplate (nav, footers, accessibility widgets). Use URL-pattern filters to focus on program pages.
- **Cross-reference externally.** The crawl gives you what the government says publicly. Supplement with news, policy docs, and people who've worked with the institution.
- **Use plan mode first.** Explore the data before committing to a report structure—ask questions, test hypotheses, change direction without generating artifacts.
- **Write in the audience's language.** Our report is in Spanish because the call is with Colombian officials.
