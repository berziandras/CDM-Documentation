---
title: Patient
layout: default
parent: UDMEDSOL
nav_order: 1
---

# Table name: PATIENT

## Reading from MEDSOL.patient and UDMED.patient

Combines and harmonizes medical patient records from two hospital information systems — MEDSOL and UDMED — into a single unified dataset. For demographic attributes where the "true" or most recent value is unknown, `MIN()` is used as a deterministic tiebreaker to consistently pick one value.

Excludes a specific patient record associated with **UDMED patients who have a TAJ number of 0** (a Hungarian social security / health insurance number). These are likely placeholder or invalid registrations and are excluded to prevent corrupt data from entering downstream models.

|     Destination Field    |     Source field     | Logic | Comment field |
|:------------------------:|:--------------------:|:--------:| |
| patient_id               | patient_id           | | |
| year_of_birth            | year_of_birth        | `MIN()` | |
| sex                      | sex                  | `MIN()` | |
| nationality              | nationality          | `MIN()` | |
| zip                      | zip                  | `MIN()` | |

## Change log
