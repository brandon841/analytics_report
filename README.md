# Analytics Report

A data analytics project for analyzing event and user invite data from BigQuery tables.

## Overview

This project queries Firebase event and user invite data from BigQuery and generates visualizations and metrics to understand:

- Number of events created per day with invites
- Accepted invites per event
- Biggest events in recent periods
- Event trends over time

## Setup

### Prerequisites

- Python 3.x
- Google Cloud BigQuery access
- Service account credentials JSON file

### Installation

1. Clone the repository
2. Install required packages:
   ```bash
   pip install pandas numpy plotly google-cloud-bigquery db-dtypes
   ```

3. Place your BigQuery service account credentials JSON file in the project directory

### Configuration

Update the BigQuery project and dataset names in `event_analytics.ipynb` to match your configuration.

## Usage

Open `event_analytics.ipynb` in Jupyter or VS Code and run the cells to:

1. Initialize the BigQuery client
2. Query events and user invites data
3. Generate analytics and visualizations

## Files

- `event_analytics.ipynb` - Main analysis notebook
- `utilities.py` - Helper functions for BigQuery initialization
- `etl-testing-478716-c0b6c2c512e0.json` - Service account credentials

## Analytics Included

- Events created per day with invites (bar chart)
- Accepted invites per event over time (line/bar chart)
- Biggest event comparison between periods (indicator visualization)
