---
title: Visit occurrence
layout: default
parent: UDCD
nav_order: 4
---

# CDM Table name: VISIT_OCCURRENCE

## Reading from UDMEDSOL.medical_case

Take all visit occurrences from UDMEDSOL table `medical_case`. Filters out visits whose `visit_end_date` extends beyond the last recorded `admittance_date` in the source `medical_case` table. This ensures that no visits spanning outside the export period are included in the output.

Maps each record to a visit type (`case_type`) concept based on `profession_code`, `department_code`, and `patient_type` using the following priority order:
- Emergency visits are identified first via `profession_code LIKE '46%'`, then split into inpatient (`ERIP`) or non-inpatient (`ER`) based on `patient_type`.
- Intensive care (`INT`) is detected either by specific profession codes or department codes. The department codes listed are not currently present in the source data.
- Remaining records fall through to general inpatient (`IP`), cure (`CURE`), or outpatient (`OP`) classification based solely on `patient_type`.

The construction of the visit_occurrence table involves 3 main steps:

1. Separate Inpatient and Outpatient Cases

    - `ip` Selects all inpatient (`'I'`) and cure (`'C'`) records from the cases table.
    - `op_during_ip` Identifies outpatient (`'O'`) records that fall entirely within an inpatient stay for the same patient — i.e. where the outpatient `admittance_datetime` is between the inpatient admission and discharge. These are considered part of the inpatient visit and are excluded from standalone outpatient records.
    - `op` Selects outpatient (`'O'`) records that are not overlapping with any inpatient stay (i.e. not present in `op_during_ip`).

2. Union

    - `unioned` Combines the filtered outpatient (`OP`) and inpatient (`IP`) records into a single dataset.
    - Visit end date handling: For outpatient records, `visit_end_date` and `visit_end_datetime` are set equal to the admission date/datetime, as discharge timestamps for outpatient cases may be unreliable. For all other types, the discharge date is used, falling back to the admission date if discharge is `NULL`.

3. Merge Overlapping Visit Intervals

    - Overlapping visit intervals are detected and grouped together, ensuring each continuous period of care is represented as a single visit occurrence.



|        Destination Field       |     Source field     | Logic | Comment field |
|:------------------------------:|:--------------------:|:--------:| |
| visit_occurrence_id            |                      | GENERATED ALWAYS AS IDENTITY | |
| person_id                      | patient_id           | JOIN person table ON person.person_source_value = medical_case.patient_id | |
| visit_concept_id               | case_type            | Map: 'IP' → 9201; 'OP' → 9202; 'CURE' → 8756; 'ERIP' → 262; 'ER' → 9203; 'INT' → 32037; ELSE → 0 | |
| visit_start_date               | admittance_date      | MIN() | |
| visit_start_datetime           | admittance_datetime  | MIN() | |
| visit_end_date                 | discharge_date       | MAX(), `admittance_date` is used for outpatients (`patient_type = 'O'`) and inpatients (`patient_type = 'I'`) if there is no `discharge_date` | |
| visit_end_datetime             | discharge_datetime   | MAX(), `admittance_datetime` is used for outpatients (`patient_type = 'O'`) and inpatients (`patient_type = 'I'`) if there is no `discharge_date` | |
| visit_type_concept_id          |                      | 32817 | |
| provider_id                    |                      | | |
| care_site_id                   | department_code      | 0 if there is no department_code | |
| visit_source_value             | case_type            | | |
| visit_source_concept_id        |                      | | |
| admitting_source_concept_id    |                      | | |
| admitting_source_value         |                      | | |
| discharge_to_concept_id        |                      | | |
| discharge_to_source_value      |                      | | |
| preceding_visit_occurrence_id  |                      | | |

## Change log
