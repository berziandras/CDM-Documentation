---
title: Diagnosis
layout: default
parent: UDMEDSOL
nav_order: 4
---

# Table name: DIAGNOSIS

## Reading from MEDSOL.diagnosis and UDMED.diagnosis

Combines and harmonizes diagnosis records from two hospital information systems — MEDSOL and UDMED — into a single unified table with a globally unique case identifier (source-prefixed to avoid ID collisions).

|       Destination Field      |      Source field     | Logic | Comment field |
|:----------------------------:|:---------------------:|:--------:| |
| case_id                      | case_id               | Prefix-based namespacing (`m` / `u`) | |
| patient_id                   | patient_id            | | |
| diagnosis_code               | diagnosis_code        | | |
| diagnosis_type               | diagnosis_type        | | |
| diagnosis_date               | diagnosis_date        | | |
| side                         | side                  | | |
| oncological_diagnosis        | oncological_diagnosis | | |
| nosonomical_stage            | nosonomical_stage     | | |

## Change log
