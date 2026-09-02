# GitHub Outages Activity Calendar

A Mercury web app for exploring GitHub incidents in a GitHub-style activity
calendar. Filter incidents by date range, impact, or affected component, then
display either the number of incidents or their total duration per day.

The example notebook is `github-incidents-calendar.ipynb`.

## Data source

The notebook loads the parsed GitHub downtime-windows dataset maintained in
the [`mrshu/github-statuses`](https://github.com/mrshu/github-statuses)
repository:

[Download `downtime_windows.csv`](https://raw.githubusercontent.com/mrshu/github-statuses/refs/heads/master/parsed/downtime_windows.csv)

Incident duration is assigned to the calendar day on which the incident
started. The source dataset is loaded directly from GitHub when the notebook
runs, so an internet connection is required.

## Run the app

From the repository root:

```bash
mercury --working-dir github-outages
```

Then open the local Mercury address shown in the terminal. See the
[Activity Calendar documentation](https://runmercury.com/docs/output/activity-calendar/)
for details about the visualization widget.
