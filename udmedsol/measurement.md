---
title: Measurement
layout: default
parent: UDMEDSOL
nav_order: 8
---

# Table name: MESUREMENT

## Reading from UDMEDSOL.measurement_glims and UDMED.measurement_udmed

Combines and harmonizes measurement records from two hospital information systems — GLIMS and UDMED — into a deduplicated table. The central challenge it addresses is that the same measurement may exist in both systems, and where overlap occurs, a deterministic rule is applied to retain exactly one record.

### GLIMS

Selects measurement records from the GLIMS export table. Key behaviours:
- Patient ID resolution. `COALESCE(mc.patient_id, mg.patient_id)` prefers the `patient_id` from the linked `medical_case` record (i.e. the UDMED-side identifier), falling back to the GLIMS-native `patient_id` if no match exists. The left join ensures GLIMS records without a matching case are still included.
- `comment` is included as a native column — GLIMS carries this field
- `case_id` is already prefixed
- Source tags are hardcoded: `source = 'GLIMS'` and `db_source = 'GLIMS_EXPORT'`
- Filter: Only records with a non-null request_id are included, anchoring each measurement to a traceable laboratory request

### UDMED

Selects measurement records from the UDMED system. Key behaviours:
- `case_id` is prefixed with `'u'` for namespacing consistency with the wider pipeline
- `comment` is hardcoded to `NULL` — UDMED measurements do not carry a comment field, so this is set explicitly to maintain structural alignment with the GLIMS
- `department_code` is sourced from `medical_case` via an inner join, meaning UDMED measurements without a linked case are excluded
- `source` is passed through from the measurement table itself, while `db_source` is hardcoded to `UDMED_EXPORT`
- Filter: Same `request_id IS NOT NULL` condition as GLIMS

### Deduplication

A measurement is considered a duplicate if it shares the same `request_id` and `mrkeyword` (the measurement keyword/code) across sources. When duplicates exist, the record from the source that sorts first alphabetically by `db_source` is kept — in practice, `GLIMS_EXPORT` sorts before `UDMED_EXPORT`, so **GLIMS is the preferred source** when both systems contain the same measurement.

### Numeric keywords (`num_mrkeywords`)

Identifies which `mrkeyword`s are predominantly numeric. For each `mrkeyword` in the deduplicated data it computes the total number of observations, the number with `data_type = 'NUM'`, and the numeric percentage. Only keywords where **more than 50%** of observations are `NUM` are kept. This keeps free-text or categorical measurements from being force-parsed as numbers just because a few of their values happen to look numeric.

### Numeric observation (`observation_num`)
The final output is all columns of the deduplicated table plus `observation_num` (`DOUBLE`), a parsed numeric form of the free-text `observation`. Parsing is attempted only for keywords in `num_mrkeywords`; for all other keywords `observation_num` is `NULL`. The value is trimmed and then parsed using the first matching rule:

1. Plain number with a dot decimal separator (e.g. `5`, `-3.2`, `+0.75`) — cast directly
2. Number starting with a comma-decimal value (e.g. `5,2` or `5,2 mmol/l`) — the leading number is extracted, the comma replaced with a dot, then cast
3. Anything else — the leading dot-decimal or integer number is extracted from the text (e.g. `5.2 mmol/l` → `5.2`) and cast

All casts use `TRY_CAST`, so values that cannot be parsed (e.g. `negative`, `<0.5`) result in `NULL` rather than an error.

|       Destination Field      |      Source field     | Logic | Comment field |
|:----------------------------:|:---------------------:|:--------:| |
| case_id                      | case_id               | Prefix-based namespacing (`m` / `u`) | |
| patient_id                   | patient_id            | | |
| examination_date             | examination_date      | | |
| mrkeyword                    | mrkeyword             | | |
| observation                  | observation           | | |
| observation_num              | observation_num       | | |
| unit                         | unit                  | | |
| normal_range                 | normal_range          | | |
| normal_minimum               | normal_minimum        | | |
| normal_maximum               | normal_maximum        | | |
| comment                      | comment               | | |
| data_type                    | date_type             | | |
| normal_status                | normal_status         | | |
| z_score                      | z_score               | | |
| zlog_score                   | zlog_score            | | |
| loinc_id                     | loinc_id              | | |
| request_id                   | request_id            | | |
| department_code              | department_code       | | |
| source                       | source                | | |
| db_source                    | db_source             | | |

## Change log
