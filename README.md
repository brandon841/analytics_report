# HeyYall Analytics Report

A comprehensive data analytics suite for analyzing user behavior, events, invitations, friendships, and churn patterns from Firebase and PostHog data in BigQuery.

## Overview

This repository contains multiple Jupyter notebooks that provide deep insights into various aspects of the HeyYall platform, including user acquisition, event creation and participation, friendship networks, churn analysis, and overall platform metrics.

## Setup

### Prerequisites

- Python 3.x
- Google Cloud BigQuery access with appropriate credentials
- Access to Firebase and PostHog datasets in BigQuery
- Environment variables configured (see Configuration section)

### Installation

1. Clone the repository
2. Install required packages:
   ```bash
   pip install pandas numpy plotly google-cloud-bigquery db-dtypes plotnine scipy python-dotenv pyarrow
   ```

3. Set up your environment variables:
   - Create a `.env` file in the project root
   - Add your BigQuery credentials path and project ID:
     ```
     BIGQUERY_CREDENTIALS_PATH=/path/to/your/credentials.json
     GOOGLE_CLOUD_PROJECT_ID=your-project-id
     ```

### Configuration

The project uses environment variables for configuration. Update the `.env` file with your specific BigQuery project and dataset information. The `utilities.py` module handles BigQuery client initialization.

## Analysis Notebooks

### 1. Event Analytics (`event_analytics.ipynb`)
Analyzes event creation patterns and invitation metrics:
- Events created per day with invites
- Accepted invites per event over time
- Biggest events in recent periods
- Event trends and participation patterns
- Event type distributions

### 2. User Analytics (`user_analytics.ipynb`)
Examines user behavior and growth patterns:
- Weekly user signups tracking
- Correlation between user signups and event creations
- User acquisition trends over time
- Platform usage patterns

### 3. Friend Analytics (`friend_analytics.ipynb`)
Explores friendship network and social connections:
- Friend graph analysis
- Network relationships between users
- Business vs. personal user connections
- Contact access patterns
- Social graph metrics

### 4. Churn Analytics (`churn_analytics.ipynb`)
Investigates user retention and churn:
- Temporal churn analysis (date-range based queries)
- User engagement patterns
- Retention metrics
- PostHog event data integration
- Churn prediction indicators

### 5. Previous Joiners Analytics (`previous_joiners_analytics.ipynb`)
Analyzes returning users and event participation:
- Network analysis of event participants
- User invitation and acceptance patterns
- Event joiner behavior
- Cross-event participation trends

### 6. Session Analytics (`session_analytics.ipynb`)
Analyzes user session behavior and engagement:
- Session duration distributions
- Session frequency and recency
- Patterns in session start and end times
- Session-based retention and activity metrics

### 6. Global Analytics (`global_analytics.ipynb`)
Provides high-level platform metrics and cross-platform comparisons:
- Total user counts (iOS vs. Web)
- Platform-wide event statistics
- Overall growth trends
- Cross-platform comparison

## Project Structure

```
analytics_report/
├── churn_analytics.ipynb           # User retention and churn analysis
├── event_analytics.ipynb           # Event creation and participation metrics
├── friend_analytics.ipynb          # Social network and friendship analysis
├── global_analytics.ipynb          # Platform-wide metrics
├── previous_joiners_analytics.ipynb # Returning user analysis
├── user_analytics.ipynb            # User growth and behavior analysis
├── utilities.py                    # BigQuery initialization utilities
├── churn_data_infrastructure.md    # Data infrastructure documentation
├── etl-testing-478716-c0b6c2c512e0.json # Service account credentials
└── README.md                       # This file
```

## Usage

1. Ensure your BigQuery credentials and environment variables are properly configured
2. Open any notebook in Jupyter or VS Code
3. Run the cells sequentially to:
   - Initialize the BigQuery client
   - Query relevant datasets
   - Generate visualizations and analytics
   - Export results as needed

Each notebook is self-contained and can be run independently based on your analysis needs.

## Data Sources

The analytics pull from multiple BigQuery datasets:
- `firebase_etl_prod.events` - Event data
- `firebase_etl_prod.userinvites` - User invitation data
- `firebase_etl_prod.users` - User profile data
- `firebase_etl_prod.friends` - Friendship connections
- `posthog_etl.events` - PostHog event tracking data

## Visualizations

The notebooks use multiple visualization libraries:
- **Plotly Express** - Interactive charts and dashboards
- **Plotnine** - Grammar of graphics style plots
- **Pandas** - Data manipulation and basic plotting

## Contributing

When adding new analysis notebooks:
1. Follow the existing naming convention
2. Include clear markdown cells explaining each analysis section
3. Use the `utilities.py` module for BigQuery initialization
4. Document any new dependencies in this README

## Security Note

**Important:** Never commit the service account credentials JSON file to version control. Ensure `etl-testing-478716-c0b6c2c512e0.json` is in your `.gitignore` file.
