---
title: Medication
layout: default
parent: UDMEDSOL
nav_order: 5
---

# Table name: MEDICATION

## Reading from MEDSOL.medication and UDMED.medication

Combines and harmonizes medication records (outpatient) from two hospital information systems — MEDSOL and UDMED — into a single unified table with a globally unique case identifier (source-prefixed to avoid ID collisions).

|       Destination Field      |      Source field     | Logic | Comment field |
|:----------------------------:|:---------------------:|:--------:| |
| case_id                      | case_id               | Prefix-based namespacing (`m` / `u`) | |
| patient_id                   | patient_id            | | |
| ttt_code                     | ttt_code              | | |
| medication_status            | medication_status     | | |
| medication_type              | medication_type       | | |
| from_date                    | from_date             | | |
| last_date                    | last_date             | | |
| atc                          | atc                   | | |
| medication_code_source_value | medication_code_source_value | | |
| description                  | description           | | |

## Change log
