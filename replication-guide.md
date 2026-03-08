# Replication guide: government program research with Firecrawl + Claude Code

A nonprofit preparing for a government meeting needs to know: which programs align with ours, what terminology to use, and how to formally partner. We automated the research phase using a web crawler and an AI coding assistant. The whole process took about an hour.

## Step 1: crawl with Firecrawl

We used [Firecrawl](https://www.firecrawl.dev/) to crawl `icbf.gov.co` from the homepage—100 pages, output as markdown (one `.md` file per page). This captured the main program descriptions and org structure but missed deeper operational manuals. A URL-pattern filter or higher page limit would improve coverage.

## Step 2: analyze with Claude Code

We opened Claude Code in the crawled-files directory and used [plan mode](https://docs.anthropic.com/en/docs/claude-code/how-to#use-plan-mode-for-complex-tasks) to explore without modifying files. This was the entire prompt:

> I'm making a demo for downloading ICBF pages using firecrawl, then using Claude Code to explore them and draft a report with findings. In this case, I'm helping Laura, the founder of ACTRA.ngo (dispatch an agent to get context on them, their program and mission) to prepare for a call with people from ICBF. She wants to know where ACTRA's program could fit with them and the exact terminology to use, so she can make concrete statements and requests. In addition to that report, I want you to create a separate report for replicating what I did, which should include some instructions on downloading the ICBF markdown from https://www.firecrawl.dev/playground?endpoint=crawl&url=www.icbf.gov.co%2F&limit=100 (not sure if 100 pages was enough/I should have used their natural language filter to select the pages better), this prompt, so she knows roughly how much input I gave (basically none) mentioning I used planning mode, and maybe any other comment that would help her replicate this.
>
> Once you're done, publish everything to a public repo and give me the link so I can share it with her

From that single prompt, Claude Code searched the crawled files for keywords, read ~10 key pages, identified three entry points and the EPRE contracting mechanism, then wrote the full Spanish report with direct quotes and ready-to-use phrases.

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
