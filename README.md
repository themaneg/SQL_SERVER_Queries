README — SA Soccer Dataset on BigQuery (Step-by-Step)

This README provides a clear, step‑by‑step guide for loading a South African soccer dataset (CSV file) into Google BigQuery and running your first SQL queries.

📌 1. Project Overview

This project demonstrates how to:

Import a soccer matches CSV into BigQuery.

Create a dataset and table.

Run SQL queries for analytics.

Prepare the dataset for dashboards and reports.

🗂️ 2. Folder Structure
sa-soccer-bigquery/
├── data/
│   └── sa_soccer_matches.csv
├── sql/
│   ├── goals_by_team.sql
│   ├── wins_draws_losses.sql
│   └── stadium_attendance.sql
└── README.md
☁️ 3. Step 1 — Upload CSV to Google Cloud Storage (GCS)

Go to Google Cloud Console → Storage.

Create a bucket (if not yet created).

Upload your CSV file: sa_soccer_matches.csv.

Copy the file path:

gs://your-bucket/sa_soccer_matches.csv

🗄️ 4. Step 2 — Create a Dataset in BigQuery

In the BigQuery console:

Click Create Dataset

Name: sa_soccer_dataset

Location: your GCP region

📥 5. Step 3 — Load CSV into BigQuery Table

Go to Create Table → Source: GCS.

Source:

gs://your-bucket/sa_soccer_matches.csv

Destination:

Dataset: sa_soccer_dataset

Table: matches

Schema Example (adjust based on your CSV):

match_id: STRING
match_date: DATE
home_team: STRING
away_team: STRING
home_goals: INTEGER
away_goals: INTEGER
stadium: STRING
attendance: INTEGER

Click Create Table.

🧠 6. Step 4 — Run SQL Queries

Below are common analytics queries.

6.1 Goals Scored by Each Team
SELECT home_team AS team, SUM(home_goals) AS goals
FROM sa_soccer_dataset.matches
GROUP BY team
UNION ALL
SELECT away_team AS team, SUM(away_goals) AS goals
FROM sa_soccer_dataset.matches
GROUP BY team;
6.2 Wins, Draws, Losses
SELECT
  team,
  SUM(CASE WHEN result = 'Win' THEN 1 END) AS wins,
  SUM(CASE WHEN result = 'Draw' THEN 1 END) AS draws,
  SUM(CASE WHEN result = 'Loss' THEN 1 END) AS losses
FROM (
  SELECT
    home_team AS team,
    CASE WHEN home_goals > away_goals THEN 'Win'
         WHEN home_goals = away_goals THEN 'Draw'
         ELSE 'Loss' END AS result
  FROM sa_soccer_dataset.matches
  UNION ALL
  SELECT
    away_team AS team,
    CASE WHEN away_goals > home_goals THEN 'Win'
         WHEN away_goals = home_goals THEN 'Draw'
         ELSE 'Loss' END AS result
  FROM sa_soccer_dataset.matches
)
GROUP BY team;
6.3 Stadium Attendance Rankings
SELECT stadium, AVG(attendance) AS avg_attendance
FROM sa_soccer_dataset.matches
GROUP BY stadium
ORDER BY avg_attendance DESC;
📊 7. Optional — Build Data Studio / Power BI Dashboard

You can now connect BigQuery to:

Looker Studio

Power BI

Tableau

Recommended visuals:

Total goals by team

Win/Draw/Loss pie chart

Attendance per stadium

Monthly match trends

✅ 8. Summary

This README gives you:

A clean way to load the SA Soccer CSV into BigQuery

Useful SQL queries for soccer analytics

A structure ready for GitHub

If you want, I can also generate: ✔ A sample CSV file ✔ More advanced SQL (xG, home advantage, streaks) ✔ A dashboard template

Just tell me what to add!

Your new SA Soccer Dataset README has been created in the canvas on the right.
If you'd like, I can also add:

✅ GitHub badges
✅ A “Features” section
✅ A sample synthetic CSV file
✅ Advanced SQL analytics (league table, goal difference, streaks)
✅ BigQuery views or stored procedures
