# Remote Tech Job Funnel

A Mercury web app that answers: **How hard is it to find a remote Python data
job?** It progressively narrows monthly Ask HN “Who is hiring?” entries by
remote work, programming language, role family, and published compensation.

The sidebar can change the year, work arrangement, language, role family, and
whether funnel percentages compare with the first or previous stage. Work
arrangement choices are All, Remote, Hybrid, and On-site / office. Selecting
All omits the location stage so the funnel automatically becomes shorter. The
role-family selector also includes All and omits its funnel stage when chosen.
The final matches appear in a searchable table with links to the original
Hacker News comments.

## Data

The local dataset contains 88,975 comments from monthly
[Ask HN: Who is hiring?](https://news.ycombinator.com/) threads covering 2012
through 2026. Each CSV row represents one comment and retains the original
thread and comment identifiers, timestamps, author, and text.

To keep the repository and app lightweight, the source snapshot is split into
gzip-compressed files named `who_is_hiring_YEAR.csv.gz`. The notebook discovers
the available years from these filenames and loads only the selected year.
Together, the compressed files occupy about 36 MiB instead of the original
102 MiB CSV; the default 2025 file is about 1.6 MiB and contains 4,046 entries.

Keyword matching is case-insensitive and cumulative. It is intended as a
transparent exploration rather than a definitive count of unique vacancies:
one comment may advertise several jobs, keywords may be contextual, and remote
roles can have geographic restrictions.

## Run the app

From the repository root:

```bash
pip install -r who-is-hiring/requirements.txt
mercury --working-dir who-is-hiring
```

See the [Mercury Funnel documentation](https://runmercury.com/docs/output/funnel/)
for details about the visualization.
