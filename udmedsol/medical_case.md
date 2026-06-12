---
title: Medical case
layout: default
parent: UDMEDSOL
nav_order: 2
---

# Table name: MEDICAL_CASE

## Reading from MEDSOL.medical_case and UDMED.medical_case

Combines and harmonizes medical case records from two hospital information systems — MEDSOL and UDMED — into a single unified dataset, with standardized department codes, deduplicated department metadata, and parsed datetime fields:
 - A globally unique case identifier (source-prefixed to avoid ID collisions)
 - Standardized and deduplicated department codes and descriptions
 - Properly typed admission and discharge timestamps
 - A traceable link back to the originating source system

UDMED is the preferred source for department metadata.

|       Destination Field      |     Source field     | Logic | Comment field |
|:----------------------------:|:--------------------:|:--------:| |
| case_id                      | case_id              | Prefix-based namespacing (`m` / `u`) | |
| patient_id                   | patient_id           | | |
| department_code              | department_code      | Trim + underscore removal before joining, full outer join with UDMED-preferred coalescing, `ROW_NUMBER()` deduplication | |
| patient_type                 | patient_type                  | | |
| case_status                  | case_status                   | | |
| admittance_date              | admittance_date               | | |
| admittance_time              | admittance_time               | | |
| admittance_datetime          | admittance_datetime           | | |
| admittance_status            | admittance_status             | | |
| admittance_type              | admittance_type               | | |
| discharge_date               | discharge_date                | | |
| discharge_time               | discharge_time                | | |
| discharge_datetime           | discharge_datetime            | | |
| discharge_type               | discharge_type                | | |
| profession_code              | profession_code               | | |
| department_code_source_value | department_code_source_value  | Original raw department code | |

## Change log
