---
title: Person
layout: default
parent: UDCD
nav_order: 1
---

# CDM Table name: PERSON

## Reading from UDMEDSOL.patient

The patients in the CDM are restricted to the subset of all UDMEDSOL patients who have at least one recorded case. Patients without at least one record in the `medical_case` table are excluded from the CDM. 

|     Destination Field    |     Source field     | Logic | Comment field |
|:------------------------:|:--------------------:|:--------:| |
| person_id                |                      | GENERATED ALWAYS AS IDENTITY | |
| gender_concept_id        | sex                  | Map: 'M' → 8507; 'F' → 8532; ELSE → 0 | |
| year_of_birth            | year_of_birth        | | |
| month_of_birth           |                      | | |
| day_of_birth             |                      | | |
| birth_datetime           |                      | | |
| race_concept_id          |                      | 0 | |
| ethnicity_concept_id     |                      | 0 | |
| location_id              | location.location_id | | |
| provider_id              |                      | | |
| care_site_id             |                      | | |
| person_source_value      | patient_id           | | |
| gender_source_value      | sex                  | | |
| gender_source_concept_id |                      | | |
| race_source_value        |                      | | |
| race_source_concept_id   |                      | | |
| ethnicity_source_value   |                      | | |

## Change log
