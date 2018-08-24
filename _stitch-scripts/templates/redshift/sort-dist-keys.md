---
title: "Stitch Scripts: Redshift SORT and DIST Keys Template"
permalink: /stitch-scripts/templates/redshift/sort-dist-keys

content-type: "template"

template-for: "redshift"
destination-name: "Redshift"

summary: |
  Rebuilds an existing table in a {{ template.destination-name }} destination with SORT and DIST key definitions, preserving table comments.

content: |
  This template will perform the following:

  1. Rebuild an existing table with SORT and DIST key definitions, preserving table comments.

  2. While the table is rebuilt, existing data is copied from the table into a temporary table.

  3. After the keys are defined on the new table, data is loaded from the temporary table back into the new table. 

  4. Ownership of the new table is transferred to the Stitch user.

  5. The temporary table is dropped.
  
---
{% assign template = page %}