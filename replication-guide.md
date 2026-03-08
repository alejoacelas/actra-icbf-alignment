# Replication guide: government program research with Firecrawl + Claude Code

This guide shows how we produced ACTRA's strategic alignment report for ICBF in about an hour, using a web crawler and an AI coding assistant.

## What this demonstrates

A nonprofit preparing for a government meeting needs to know: which programs align with ours, what terminology to use, and how to formally partner. Normally this means days of reading government websites, asking contacts, and guessing at bureaucratic structure. We automated the research phase.

## Step 1: crawl the government website with Firecrawl

We used [Firecrawl](https://www.firecrawl.dev/) to crawl `icbf.gov.co`, starting from the homepage.

**Settings:**
- Starting URL: `https://www.icbf.gov.co`
- Page limit: 100 pages
- Output format: Markdown (one `.md` file per page)
- Crawl mode: default (follows internal links from the starting URL)

**Output:** ~100 markdown files in a directory named with a Firecrawl job UUID (`019cce27-9758-700c-9ee7-78f77f48bd19/`). Each file is named after the URL path (e.g., `www.icbf.gov.co_programas-y-estrategias_direccion-de-adolescencia-y-juventud.md`).

**Limitations:**
- 100 pages captured the main program descriptions, organizational structure, and some policy documents, but missed deeper operational manuals and SRPA-specific pages.
- Government sites have heavy boilerplate (navigation, footers, accessibility widgets) that inflates file sizes without adding information.
- A natural-language filter (e.g., "only keep pages about youth programs") would improve signal-to-noise ratio.

## Step 2: analyze with Claude Code

We opened Claude Code in the directory containing the crawled files and used [plan mode](https://docs.anthropic.com/en/docs/claude-code/how-to#use-plan-mode-for-complex-tasks) to explore without modifying files. The conversation went roughly like this:

1. **Orientation:** "I have ~100 crawled pages from icbf.gov.co. ACTRA is a nonprofit that does group CBT for crime prevention with adolescents in Bogotá. Help me find where ACTRA's program fits within ICBF's structure."

2. **Exploration:** Claude Code searched the crawled files for keywords (prevención del delito, adolescentes, TCC, responsabilidad penal, organizaciones aliadas) and read the relevant pages.

3. **Synthesis:** After reading ~10 key pages, Claude Code identified three entry points (Línea 11, SRPA, Somos Familia), the EPRE mechanism for partnering, and the terminology mappings.

4. **Report generation:** We exited plan mode and asked Claude Code to write the full report in Spanish, with direct quotes from the source files and ready-to-use phrases for the call.

The entire analysis—from first prompt to final report—took one conversation session.

## Step 3: publish

```bash
cd /path/to/project
git init
git add .
git commit -m "ACTRA-ICBF strategic alignment report"
gh repo create actra-icbf-alignment --public --source=. --push
```

## Tips for replicating this approach

- **Crawl more pages with a filter.** 100 pages is a reasonable starting point, but for a large government site you'll miss important subpages. Firecrawl supports filtering by URL pattern—use it to focus on program pages.
- **Government sites have heavy boilerplate.** Expect 30–50% of each page to be navigation, footers, and accessibility widgets. The AI handles this fine but it inflates token costs.
- **Cross-reference with external context.** The crawl gives you what the government says publicly. Supplement with news articles, policy documents, and conversations with people who've worked with the institution.
- **Use plan mode first.** Exploring in plan mode lets you understand the data before committing to a report structure. You can ask questions, test hypotheses, and change direction without generating artifacts prematurely.
- **Write the report in the audience's language.** Our report is in Spanish because Laura's call is with Colombian officials. The AI handles multilingual output natively.
