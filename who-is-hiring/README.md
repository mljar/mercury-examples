# Who Is Hiring

Two Mercury notebook apps explore comments from monthly Ask HN “Who is
hiring?” threads:

- [`who-is-hiring-funnel.ipynb`](who-is-hiring-funnel.ipynb) — an interactive
  funnel showing how search requirements reduce the available opportunities.
- [`language-mentions-sankey.ipynb`](language-mentions-sankey.ipynb) — a Sankey
  comparison of programming-language mentions across selected years.

## Remote Tech Job Funnel

The funnel app answers questions such as: **How hard is it to find a remote
Python data job?** It starts with every entry in the selected year, then
progressively applies the chosen work arrangement, programming language, role
family, and published-compensation requirements.

The sidebar can change the year, work arrangement, language, role family, and
whether funnel percentages compare with the first or previous stage. Work
arrangement choices are All, Remote, Hybrid, and On-site / office. Selecting
All omits the location stage so the funnel automatically becomes shorter. The
language and role-family selectors also include All and omit their respective
stages when chosen. The title, indicators, funnel stages, explanatory text, and
searchable results table update with the selections. Each result links to the
original Hacker News comment.

The default 2025 remote + Python + Data / ML search narrows 4,046 entries to 61
entries that also mention salary or compensation. These counts use transparent
keyword matching and are not a claim about unique vacancies.

See the [Mercury Funnel documentation](https://runmercury.com/docs/output/funnel/)
for widget details.

## Programming Language Sankey

The Sankey app compares programming-language mentions in selected years. Its
default view uses 2012, 2018, and 2026, with controls for years, languages, and
the ribbon metric:

- **Share of tracked mentions (%)** shows a language's share of all selected
  language mentions within each year.
- **Post mentions** shows the number of distinct posts mentioning the language.

A language is counted at most once per post, but one post can contribute to
several languages. Languages mentioned in fewer than a hard-coded 2% of a
year's posts are hidden to keep the chart readable. Change
`MIN_POST_SHARE_PCT` in the notebook to adjust that cutoff.

The diagram represents **year → language mention** relationships. It does not
track the same jobs over time and must not be interpreted as one language
turning into another. Because all selected years connect to a shared language
node, the number shown beside that node is the sum of its incoming ribbons. For
example, a Python node above 100 in percentage mode is a total across years,
not a single-year percentage. The table below the chart contains the individual
year values.

See the [Mercury Sankey documentation](https://runmercury.com/docs/output/sankey/)
for widget details.

## Data

The local dataset contains 88,975 comments from monthly
[Ask HN: Who is hiring?](https://news.ycombinator.com/) threads covering 2012
through 2026. Each CSV row represents one comment and retains the original
thread and comment identifiers, timestamps, author, and text.

To keep the repository and apps lightweight, the source snapshot is split into
gzip-compressed files named `who_is_hiring_YEAR.csv.gz`. Both notebooks discover
the available years from these filenames. The funnel loads only one selected
year; the Sankey loads only the selected comparison years. Together, the files
occupy about 36 MiB instead of the original 102 MiB CSV. The default 2025 file
is about 1.6 MiB and contains 4,046 entries. The 2026 file is a partial-year
snapshot.

Keyword matching is case-insensitive and cumulative. It is intended as a
transparent exploration rather than a definitive count of unique vacancies:
one comment may advertise several jobs, keywords may be contextual, and remote
roles can have geographic restrictions.

## Run the apps

From the repository root:

```bash
pip install -r who-is-hiring/requirements.txt
mercury --working-dir who-is-hiring
```

Mercury discovers both notebooks and publishes them as separate apps.
