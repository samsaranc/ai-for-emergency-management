# AI for Emergency Management Resources

A single-aesthetic Quarto site that turns *Investigating the Role of AI in Emergency Management* (FAccT '26) into a project picker and onboarding kit:

- **Use-Case Catalog** — all 136 use cases, filterable by category, EM phase, feasibility, and current AI use, each with an "open problems" column.
- **Datasets / Domain Literacy / Meet an EM / Anti-Patterns / Instruments** — the onboarding bundle, grounded in the paper.

Live site: <https://samsaranc.github.io/ai-for-emergency-management/>
Repo: <https://github.com/samsaranc/ai-for-emergency-management>

The catalog uses **Observable JS**, which renders in the browser — so the whole site builds with **Quarto alone, no R or Python runtime**. That keeps both local setup and CI deployment simple.

---

## Quick start

1. **Install Quarto** — <https://quarto.org/docs/get-started/> (one installer, all platforms).
2. **Preview locally** from the project folder:
   ```bash
   quarto preview
   ```
   This opens a live-reloading local server. Edit any `.qmd` and the page refreshes.
3. **Render the static site** to `_site/`:
   ```bash
   quarto render
   ```

No `renv`, no virtualenv, no package install. If `quarto preview` works, you're done.

## Deploy to GitHub Pages

The repo already exists at `samsaranc/ai-for-emergency-management`. From the unzipped project folder:

```bash
git init
git add .
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/samsaranc/ai-for-emergency-management.git
git push -u origin main

quarto publish gh-pages   # renders, creates gh-pages, pushes, opens the live site
```

That first local `quarto publish gh-pages` also creates the `_publish.yml` config the GitHub Action needs. After that, every `git push` to `main` auto-rebuilds and republishes via `.github/workflows/publish.yml`.

If the site 404s after a minute or two, set **Settings -> Pages -> Source: Deploy from a branch -> `gh-pages` -> `/root`**. If the auto-build fails, set **Settings -> Actions -> General -> Workflow permissions -> Read and write permissions**.

### Prefer OSF?
Render locally (`quarto render`) and upload the contents of `_site/` to an OSF project, or deposit the `instruments/` files there for a citable DOI and link to them from `instruments.qmd`.

## Edit the catalog

The catalog reads `data/use_cases.csv`. Columns:

| column | meaning |
|---|---|
| `id` | stable identifier, e.g. `UC1-04` |
| `use_case` | short title |
| `category` | one of the 7 categories (drives the Category filter) |
| `phase` | `Mitigation` / `Preparedness` / `Response` / `Recovery`, slash-separated for multiple (drives phase chips + filter) |
| `participants` | # of interviewees who raised the category |
| `feasibility` | `High` / `Moderate` / `Mixed` / `Low` (color-coded; drives filter) |
| `current_ai` | `Yes` / `Limited` / `None` |
| `datasets_methods` | relevant datasets, models, methods |
| `open_problems` | the research-agenda column |

The file ships with **20 grounded example rows across all 7 categories**. Add rows to reach the full 136 — the page updates automatically, and the filters/chips/colors work with no code changes. Wrap any cell containing a comma in double quotes.

## Swap the catalog engine (optional)

Observable JS is the default because it needs no language runtime. If you'd rather use a familiar stack:

- **Python (itables):** add a `{python}` cell — `from itables import show; import pandas as pd; show(pd.read_csv("data/use_cases.csv"))`. Requires a Jupyter kernel with `itables` + `pandas`, and a Python setup step in the CI workflow.
- **R (reactable):** add an `{r}` cell — `reactable::reactable(read.csv("data/use_cases.csv"), searchable = TRUE, filterable = TRUE)`. Requires R + `reactable`, and an R setup step in CI.

Both give nice filtering; both reintroduce a runtime dependency, which is why the default avoids them.

## Files

```
_quarto.yml          site config (navbar, theme, footer)
styles.scss          the "EOC status-board" theme (colors, fonts, hero, table)
index.qmd            hero + start-here cards
catalog.qmd          filterable 136-use-case catalog (Observable JS)
datasets.qmd         datasets starter pack + bias notes
literacy.qmd         NIMS/ICS, Whole Community, EMAP, evidence base
meet-an-em.qmd       ethical practitioner engagement
anti-patterns.qmd    the rabbit holes
instruments.qmd      protocol + codebook for replication
data/use_cases.csv   catalog data (extend to 136)
preview.html         standalone, no-install preview of the hero + catalog
.github/workflows/publish.yml   render + deploy
```

## TODOs left for you

Search the pages for `TODO:` — they mark the spots to add real links (dataset repos, FEMA docs, ICS-201/IAP samples, your IRB resources) and to drop in the protocol/codebook files.

---

Text is offered CC BY 4.0, matching the paper. Built with [Quarto](https://quarto.org).
