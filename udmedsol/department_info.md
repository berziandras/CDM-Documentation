---
title: Department info
layout: default
parent: UDMEDSOL
nav_order: 3
---

# Table name: DEPARTMENT_INFO

## Reading from MEDSOL.department_info and UDMED.department_info

Combines and harmonizes department metadata from two hospital information systems — MEDSOL and UDMED — into a single unified table with standardized and deduplicated department codes and descriptions.

UDMED is the preferred source for department metadata.

|       Destination Field      |     Source field     | Logic | Comment field |
|:----------------------------:|:--------------------:|:--------:| |
| department_code              | department_code      | Trim + underscore removal before joining, full outer join with UDMED-preferred coalescing, `ROW_NUMBER()` deduplication | |
| description                  | description          | | |
| department_type_code         | department_type_code | | |

## Change log
