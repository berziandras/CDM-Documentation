---
title: Treatment
layout: default
parent: UDMEDSOL
nav_order: 6
---

# Table name: TREATMENT

## Reading from MEDSOL.treatment and UDMED.treatment

Combines and harmonizes treatment records from two hospital information systems — MEDSOL and UDMED — into a single unified table with a globally unique case identifier (source-prefixed to avoid ID collisions). Deduplicates the `description` column so that each `oeno_code` maps to exactly one description.

The treatment date uses `COALESCE(treatment_date, mc.admittance_date)` — treatment-level date is preferred when available, falling back to the case's admittance date if not. This reflects that UDMED captures more granular date information than MEDSOL.

|       Destination Field      |      Source field     | Logic | Comment field |
|:----------------------------:|:---------------------:|:--------:| |
| case_id                      | case_id               | Prefix-based namespacing (`m` / `u`) | |
| patient_id                   | patient_id            | | |
| quantity                     | quantity              | | |
| oeno_code                    | oeno_code             | | |
| treatment_date               | treatment_date        | | |
| treatment_code_source_value  | treatment_code_source_value | | |
| description                  | description           | | |

## Change log
