---
title: UDMEDSOL to EVENT_COMBINED (STEM)
layout: default
parent: UDCD
nav_order: 8
has_children: true
---

# CDM Table name: EVENT_COMBINED

The `EVENT_COMBINED` (`STEM`) table is a staging area where UDMEDSOL source codes will first be mapped to concept_ids. The `STEM` table itself is an amalgamation of the OMOP event tables to facilitate record movement. This means that all fields present across the OMOP event tables are present in the `STEM` table. After a record is mapped and staged, the domain of the concept_id dictates which OMOP table (Condition_occurrence, Drug_exposure, Procedure_occurrence, Measurement, Observation, Device_exposure) the record will move to.