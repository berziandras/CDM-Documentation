---
title: Observation period
layout: default
parent: UDCD
nav_order: 5
---

# CDM Table name: OBSERVATION_PERIOD

## Reading from VISIT_OCCURRENCE

Take all records from the `visit_occurrence` table and create one `observation_period` record for each `person_id`. The observation period starts at the earliest `visit_start_date` and ends at the latest `visit_end_date` across all visits. If the latest discharge date is missing or earlier than the latest admission date, the latest admission date is used as the end date instead.

|        Destination Field      |     Source field     | Logic | Comment field |
|:-----------------------------:|:--------------------:|:--------:| |
| observation_period_id         |                      | `GENERATED ALWAYS AS IDENTITY` | |
| person_id                     | person_id            | | |
| observation_period_start_date | visit_start_date     | `MIN()` | |
| observation_period_end_date   | visit_end_date       | `MAX()`, if the latest discharge date is missing or earlier than the latest admission date, the latest admission date is used as the end date instead. | |
| period_type_concept_id        |                      | Use 32817 - EHR | |

## Change log
