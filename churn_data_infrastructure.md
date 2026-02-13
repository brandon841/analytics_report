# Churn Calculation Data Model Foundation for an Event Planning Application

## Overview

This document outlines the best practices and recommended data structures for calculating churn and resurrection (reactivation) in your event planning app. It is tailored for an app where users can host ticketed, fundraiser, or free events, as well as perform various social media interactions like check-ins.

---

## Churn Definition Strategies

**Multiple churn definitions are recommended due to diverse user behaviors:**

### 1. Time-Based Inactivity
- A user is considered churned after X days of inactivity (common thresholds: 30, 60, 90 days).
- Different user personas (hosts, attendees, social users) may require different thresholds.

### 2. Behavioral Churn
- Define churn based on "core value actions":
  - **Host churn:** No event creation in 90 days.
  - **Social churn:** No social interaction (check-in, comment, share) in 30 days.
  - **Attendee churn:** No event registration or attendance in 60 days.

### 3. Cohort-Based Approach
- Track churn by user cohorts (e.g., new hosts vs. repeat hosts, free vs. paid users).

---

## Resurrection (Reactivation) Handling

Track all state transitions:
- Initial activation date
- Last active date
- Churn date (when inactivity threshold is crossed)
- Reactivation date (when user comes back)
- Number of churn cycles

Key metrics:
- Net churn rate = (Churned users - Reactivated users) / Total active users
- Resurrection rate = Reactivated users / Churned users

---

## Recommended Data Model

### 1. **Activity Log Table** (Fact Table)
Record every user action as a row.

```sql
activity_log
    user_id
    activity_date (timestamp)
    activity_type (event_created, ticket_purchased, check_in, etc.)
    event_id (nullable)
    additional_metadata (JSON or more columns)
```

### 2. **User Daily Activity Table** (Materialized View or Daily Job)
Aggregate actions per user per day.

```sql
user_daily_activity
    user_id
    activity_date
    total_events_created
    total_tickets_purchased
    total_social_interactions
    last_activity_timestamp
    has_any_activity (boolean)
```

### 3. **User Churn State Table** (Key for Churn Tracking)
Track each user’s current state (active, at risk, churned, reactivated).

```sql
user_churn_state
    user_id
    current_state ('active', 'at_risk', 'churned', 'reactivated')
    state_since_date
    last_active_date
    first_active_date
    churn_date (nullable)
    times_churned
    times_reactivated
    last_meaningful_action
    user_segment (host, attendee, social, hybrid)
```

#### Typical State Transition Flow
```
Active → Churned → Reactivated → (Active/Churned again)
```

---

## Example Query Patterns

### Monthly Churn Rate
```sql
SELECT 
  DATE_TRUNC('month', state_since_date) AS month,
  COUNT(*) FILTER (WHERE current_state = 'churned') AS churned_users,
  COUNT(*) FILTER (WHERE current_state IN ('active', 'at_risk')) AS active_users,
  ROUND(100.0 * COUNT(*) FILTER (WHERE current_state = 'churned') / 
        NULLIF(COUNT(*), 0), 2) AS churn_rate
FROM user_churn_state
WHERE state_since_date >= '2026-01-01'
GROUP BY month
ORDER BY month;
```

### Resurrection Rate
```sql
SELECT 
  DATE_TRUNC('month', state_since_date) AS month,
  COUNT(*) FILTER (WHERE current_state = 'reactivated') AS reactivated,
  times_reactivated,
  AVG(EXTRACT(days FROM (state_since_date - churn_date))) AS avg_days_to_return
FROM user_churn_state
WHERE current_state = 'reactivated'
  AND state_since_date >= '2026-01-01'
GROUP BY month, times_reactivated
ORDER BY month;
```

### At-Risk User Identification
```sql
SELECT 
  user_id,
  MAX(activity_date) AS last_active,
  CURRENT_DATE - MAX(activity_date) AS days_inactive
FROM user_daily_activity
GROUP BY user_id
HAVING CURRENT_DATE - MAX(activity_date) BETWEEN 30 AND 60
ORDER BY days_inactive DESC;
```

---

## Implementation Steps

### Recommended Architecture

```
┌──────────────┐    Raw events
│ Activity Log │ ←─────────────
│   (Fact)     │
└──────┬───────┘
       │
    Daily ETL/Batch Jobs
       │
┌──────▼──────────┐
│ User Daily      │ ← Aggregated summaries
│ Activity Table  │
└──────┬──────────┘
       │
    Churn Calculation Job
       │
┌──────▼──────────┐
│ User Churn      │ ← Current state snapshots
│ State Table     │
└─────────────────┘
```

---

## Additional Tips

- **Indexes:** On `user_id`, `activity_date`, and `current_state` for speed.
- **Partitioning:** By month on large tables.
- **Materialized Views:** For fast dashboard reporting.
- **Flexible Definitions:** Allow churn thresholds to be easily configurable.

---

## Next Steps

- Define your activity types most important for your app's value.
- Implement the three tables above in your chosen database (PostgreSQL, MySQL, BigQuery, etc.).
- Set up daily or weekly jobs to update activity summaries and churn states.

---

If you want help designing the exact SQL schema or ETL queries for your tech stack, let me know what technology you’re using!
````I've written the information to a markdown file named `churn_data_model_foundation.md` in your workbench, so you can easily reference and update it as you progress.