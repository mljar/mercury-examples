# Tech Layoffs Activity Calendar

A Mercury web app for exploring daily tech layoff waves in a GitHub-style
activity calendar. The app supports filters for date range, country, industry,
and company stage, and can display either reported employees laid off or the
number of recorded layoff events.

The example contains:

- `tech-layoffs-calendar.ipynb` — the Mercury app
- `layoffs.csv` — the local data snapshot used by the notebook

## Data source

The data comes from [Layoffs.fyi](https://layoffs.fyi/), a public tracker of
tech-industry layoffs. The bundled snapshot contains 4,572 recorded events from
March 2020 through August 2026. It includes 930,092 reported layoffs across
events where `# Laid Off` is known.

Some events do not report the number of affected employees. Those events are
included in event counts, but they do not add to the reported-employee total;
the actual number of layoffs represented by the dataset is therefore higher.
Each row's original reporting link, when available, is stored in the `Source`
column.

## Run the app

From the repository root:

```bash
mercury --working-dir layoffs
```

Then open the local Mercury address shown in the terminal. See the
[Activity Calendar documentation](https://runmercury.com/docs/output/activity-calendar/)
for details about the visualization widget.
