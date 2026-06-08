---
title: Measurement
layout: default
parent: UDMEDSOL to EVENT_COMBINED (STEM)
nav_order: 4
---

# CDM Table name: EVENT_COMBINED

## Reading from UDMEDSOL.measurement

The `EVENT_COMBINED` (`STEM`) table is a staging area where UDMEDSOL source codes like Measurement codes will first be mapped to concept_ids. The `STEM` table itself is an amalgamation of the OMOP event tables to facilitate record movement. This means that all fields present across the OMOP event tables are present in the `STEM` table. After a record is mapped and staged, the domain of the concept_id dictates which OMOP table (Condition_occurrence, Drug_exposure, Procedure_occurrence, Measurement, Observation, Device_exposure) the record will move to.

Filters out records where `examination_date` falls after the last recorded `admittance_date` in the source `medical_case` table. This ensures that no measurements outside the export period are included in the output.

![](md_files/image2.png)

|        Destination Field      |         Source field        | Logic | Comment field |
|:-----------------------------:|:---------------------------:|:-----:|:-------------:|
| id                            |                             | `GENERATED ALWAYS AS IDENTITY` | |
| person_id                     | patient_id                  | JOIN visit_detail table ON measurement.case_id = visit_detail.visit_id_source_value | |
| concept_id                    | loinc_id                    | `COALESCE(concept_id, 0)`, LEFT JOIN concept table ON measurement.loinc_id = concept.concept_code | |
| type_concept_id               |                             | Use 32817 - EHR  | |
| start_datetime                | examination_date            | `to_timestamp()` | |
| end_datetime                  |                             |                  | |
| visit_occurrence_id           | visit_occurrence_id         | FROM visit_detail | |
| visit_detail_id               | visit_detail_id             | FROM visit_detail | |
| provider_id                   |                             |       | |
| source_value                  | mrkeyword                   |       | |
| source_concept_id             |                             |       | |
| quantity                      |                             |       | |
| unit_concept_id               | unit                        | `COALESCE(unit_concept_id, 0)` | |
| unit_source_value             | unit                        |       | |
| unit_source_concept_id        |                             |       | |
| value_as_number               | observation                 | WHEN data_type = 'NUM' THEN TRY_CAST(observation AS DOUBLE), ELSE NULL | |
| value_as_concept_id           | observation                 | Map: '%poz%' → 45884084; '%neg%' → 45878583; ELSE → NULL | |
| value_as_string               |                             |       | |
| specimen_source_id            |                             |       | |
| anatomic_site_concept_id      |                             |       | |
| anatomic_site_source_value    |                             |       | |
| disease_status_concept_id     |                             |       | |
| disease_status_source_value   |                             |       | |
| modifier_concept_id           |                             |       | |
| modifier_source_value         |                             |       | |
| verbatim_end_date             |                             |       | |
| stop_reason                   |                             |       | |
| refills                       |                             |       | |
| days_supply                   |                             |       | |
| sig                           |                             |       | |
| route_concept_id              |                             |       | |
| route_source_value            |                             |       | |
| lot_number                    |                             |       | |
| unique_device_id              |                             |       | |
| production_id                 |                             |       | |
| condition_status_concept_id   |                             |       | |
| condition_status_source_value |                             |       | |
| operator_concept_id           |                             |       | |
| value_source_value            | observation                 |       | |
| range_low                     | normal_minimum              |       | |
| range_high                    | normal_maximum              |       | |
| qualifier_concept_id          |                             |       | |
| qualifier_source_value        |                             |       | |
| reference_field_concept_id    |                             |       | |
| reference_event_id            |                             |       | |
| event_time                    |                             |       | |
| domain                        | concept.domain_id           |       | |
| default_domain                |                             | 'Measurement' | |
| amount_allowed                |                             |       | |
| cost_type_concept_id          |                             |       | |
| currency_concept_id           |                             |       | |
| drg_concept_id                |                             |       | |
| drg_source_value              |                             |       | |
| paid_by_patient               |                             |       | |
| paid_by_payer                 |                             |       | |
| paid_by_primary               |                             |       | |
| paid_dispensing_fee           |                             |       | |
| paid_ingredient_cost          |                             |       | |
| paid_patient_coinsurance      |                             |       | |
| paid_patient_copay            |                             |       | |
| paid_patient_deductible       |                             |       | |
| payer_plan_period_id          |                             |       | |
| revenue_code_concept_id       |                             |       | |
| revenue_code_source_value     |                             |       | |
| total_charge                  |                             |       | |
| total_cost                    |                             |       | |
| total_paid                    |                             |       | |
| visit_occurrence_source_value |                             |       | |
| source_table                  |                             | 'measurement' | |
| source_table_pk               |                             |       | |

## Change log
