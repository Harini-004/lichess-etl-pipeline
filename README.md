# Lichess Ratings ETL Pipeline

A small data pipeline that pulls player ratings from the free Lichess API, cleans the data, and loads it into a MySQL database. It runs automatically every day.

## What it does
1. **Extract:** calls the Lichess API for a list of players and collects username, blitz rating, bullet rating and blitz games played.
2. **Transform:** removes unreliable rows. Players with 100 or fewer blitz games, or a placeholder bullet rating of 1500, are dropped.
3. **Load:** writes the cleaned table into a MySQL database (`chess_db`, table `players`). Re-running replaces the table, so there are no duplicate rows.

Failed API requests are skipped with a message instead of crashing the run.

## Tech
Python, Requests, Pandas, SQLAlchemy, PyMySQL, MySQL, Windows Task Scheduler

## Project structure
    pipeline.py        # extract, transform and load functions
    run_pipeline.bat   # runs the pipeline and writes output to log.txt
    README.md

## How to run
1. Install the packages:

        pip install requests pandas sqlalchemy pymysql

2. Create the database in MySQL:

        CREATE DATABASE chess_db;

3. Set your MySQL password as an environment variable (never put it in the code), then open a new terminal:

        setx MYSQL_PASSWORD "your_password"

4. Run the pipeline:

        python pipeline.py

5. Check the result in MySQL:

        SELECT * FROM players ORDER BY blitz DESC;

## Scheduling
`run_pipeline.bat` runs the pipeline and appends the output to `log.txt`. It is scheduled to run daily with Windows Task Scheduler.

## Example output
    loaded 5 rows

## Possible improvements
- Keep history by appending each day's data with a date column instead of replacing the table
- Move the player list to a config file
- Add retries and failure alerts, or move scheduling to Apache Airflow
