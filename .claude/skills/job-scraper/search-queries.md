# Search Queries for Job Scraper

## Installed portal CLIs (primary for `/scrape`)

`/scrape` discovers every portal skill under `.agents/skills/*/SKILL.md` and runs its CLI first. Shipped country-agnostic CLIs include `linkedin-search` and `freehire-search`; Danish demos (`jobbank-search`, `jobdanmark-search`, `jobindex-search`, `jobnet-search`) are also installed but are Denmark-only and not relevant to this candidate's Brazil-focused search. No Brazilian portal CLI is installed yet — consider `/add-portal` for a board like Catho, Vagas.com, Gupy, or InfoJobs. You do **not** need a matching `site:` line below for a CLI to run.

The `site:` query templates in this file are the **WebSearch fallback** — for portals without a CLI, company career pages, or when a CLI fails.

**Language scope:** candidate's Languages table (CLAUDE.md) is Português (Native) and Inglês (B1/B2). Queries are written primarily in Portuguese, matching the target market (Brazil); a few English variants are included for broader-net LinkedIn/remote searches, since the CV language is Portuguese but English postings at a conversational level still pass the Language Gate.

## Search Sites

Primary (candidate's market - scaffold a CLI with `/add-portal`):
- **vagas.com.br** - large Brazilian general job board (WebSearch fallback until a CLI exists)
- **catho.com.br** - large Brazilian general job board (WebSearch fallback until a CLI exists)
- **linkedin.com/jobs** - LinkedIn job listings (filter: Brasil); also covered by the `linkedin-search` CLI
- **infojobs.com.br** - Brazilian job board, strong for industrial/operations roles (WebSearch fallback)

Secondary (company career pages via Google):
- Direct Google searches with `site:` filters for known target companies (none specified yet - ask the candidate if they want specific companies monitored)

## Query Categories

Queries are grouped by priority. Combine each query with location terms where the site supports it — but per the Location Filter below, this candidate has no location constraint, so location terms are optional/broadening rather than restrictive.

### Priority 1: Coordenação/Gerência de PCP

These match the candidate's strongest and most desired career direction.

```
site:vagas.com.br "Coordenador de PCP" Brasil
site:vagas.com.br "Gerente de PCP" Brasil
site:linkedin.com/jobs "Coordenador de PCP" Brasil
site:linkedin.com/jobs "Gerente de PCP" Brasil
"Coordenador de PCP" Brasil OR remoto OR "home office"
"Gerente de PCP" Brasil OR remoto OR "home office"
```

### Priority 2: Coordenação/Gerência de Produção ou Industrial

These match the candidate's domain expertise in production/industrial operations.

```
site:vagas.com.br "Coordenador de Produção" Brasil
site:vagas.com.br "Gerente de Produção" Brasil
site:catho.com.br "Coordenador Industrial" Brasil
site:linkedin.com/jobs "Coordenador de Produção" Brasil
site:linkedin.com/jobs "Gerente de Produção" Brasil
```

### Priority 3: Supply Chain / Planejamento (adjacent roles)

Adjacent roles the candidate could pivot into, drawing on PCP, planning, and logistics experience.

```
site:vagas.com.br "Coordenador de Supply Chain" Brasil
site:linkedin.com/jobs "Coordenador de Supply Chain" Brasil
site:infojobs.com.br "Analista de Planejamento" Sênior Brasil
"Coordenador de Logística" PCP OR planejamento Brasil
```

### Priority 4: Broader PCP / Operations / Automation (wider net)

Wider net for planning/production roles, including English-language postings for remote/multinational companies.

```
"Planejamento e Controle de Produção" vaga Brasil
site:linkedin.com/jobs "Production Planning" Manager OR Coordinator Brazil OR remote
site:linkedin.com/jobs "Production Coordinator" OR "Production Manager" Brazil
"PCP" automação OR "Power BI" coordenação OR gerência Brasil
```

## Location Filter

The candidate has **no location constraint** (see CLAUDE.md Identity / Deal-breakers): remote, hybrid, on-site, and relocation are all acceptable. Do not filter out results by distance from Colatina, ES. Note the modality (remote/hybrid/on-site) and city in the results table so the candidate can judge each on its own merits, but do not exclude on location grounds.

## Salary Filter

Deal-breaker per CLAUDE.md: minimum salary **R$ 7.000,00**. When a posting states a salary below this, flag it clearly (do not silently exclude — some postings omit salary and only reveal it later, and the candidate may still want visibility). When no salary is stated, include normally and note "salary not disclosed".

## Seniority Filter

Target seniority: Pleno, Sênior, Coordenação, Gerência. Exclude: Estágio, Trainee, Assistente de PCP, Auxiliar de Produção, Operador de Máquinas (these are below the candidate's current level).

## Language Filter

Candidate's working languages and levels are in CLAUDE.md's Languages table (Português - Native, Inglês - B1/B2). Apply `04-job-evaluation.md`'s Language Gate: a posting requiring a language not declared at all is excluded; a posting requiring a higher level than declared in a language the candidate does work in is not excluded, flag it clearly instead (see `job-scraper/SKILL.md`'s Step 3 "Quick Fit Assessment" for how the flag surfaces in `/scrape` output). Postings simply *written* in a language the candidate doesn't work in, that don't require it on the job, are fine.

## Date Filter

Only include jobs posted within the last 14 days, or with an application deadline that has not yet passed. If a posting date cannot be determined, include it but flag as "date unknown".

## Adapting Queries

If the user specifies a focus area, select queries from the matching category and also generate 2-3 custom queries for that focus. For example:
- "/scrape supply chain" -> Priority 3 queries + custom Supply Chain-specific queries
