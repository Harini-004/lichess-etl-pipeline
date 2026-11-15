# Lichess Ratings ETL Pipeline

A small data pipeline that pulls player ratings from the free Lichess API, cleans the data, and loads it into a SQLite database.

## What it does
1. **Extract:** calls the Lichess API for a list of players and collects username, blitz rating, bullet rating and blitz games played.
2. **Transform:** removes unreliable rows. Players with 100 or fewer blitz games, or a placeholder bullet rating of 1500, are dropped.
3. **Load:** writes the cleaned table into a SQLite database (`chess.db`). Re-running replaces the table, so no duplicate rows.

Failed API requests are skipped with a message instead of crashing the run.

## Tech
Python, Requests, Pandas, SQLite

## How to run
    pip install requests pandas
    python pipeline.py

## Example output
    loaded 5 rows

## Possible improvements
- Schedule it to run daily
- Store history instead of replacing the table
- Move the player list to a config file