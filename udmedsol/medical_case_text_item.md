---
title: Medical case text item
layout: default
parent: UDMEDSOL
nav_order: 7
---

# Table name: MEDICAL_CASE_TEXT_ITEM

## Reading from MEDSOL.medical_case_text_item and UDMED.medical_case_text_item

Combines and harmonizes medical text records from two hospital information systems — MEDSOL and UDMED — into a single unified table with a globally unique case identifier (source-prefixed to avoid ID collisions). MEDSOL's internal blocktype codes are normalised into a standardised vocabulary shared with UDMED.



|       Destination Field      |      Source field     | Logic | Comment field |
|:----------------------------:|:---------------------:|:--------:| |
| case_id                      | case_id               | Prefix-based namespacing (`m` / `u`) | |
| patient_id                   | patient_id            | | |
| blocktype                    | blocktype             |  Map: ('adn', 'anm', 'cpl', 'dec', 'dgt', 'epd', 'oor', 'ope', 'sta', 'sug', 'the') → 'AMBLAP'; '_dl%' → 'ZAROJEL'; ELSE → 0  | |
| text_item                    | text_item             | | |
| blocktype_source_value       | blocktype_source_value | | |

## Change log
