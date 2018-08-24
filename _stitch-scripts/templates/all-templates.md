---
title: "Stitch Scripts: Templates"
permalink: /stitch-scripts/templates

type: "template"

# If a template for a destination that isn't listed
# needs to be added, add it to `destinations` (below)
# using the display name of the destination. Ex: `BigQuery`

sections:
  - content: |
      {% assign destinations = "Redshift|BigQuery|Snowflake" | split: "|" %}

      {% for destination in destinations %}
      {% assign destination-name = destination | downcase %}
      {% assign all-template-docs = site.stitch-scripts | where:"content-type",page.type %}
      {% assign template-docs = all-template-docs | where:"template-for",destination-name %}

      ## {{ destination }}

      {% for template in template-docs %}
      ### [{{ template.title | remove: "Stitch Scripts: " }}]({{ template.url | prepend: site.baseurl }})

      {{ template.summary | flatify | markdownify }}
      {% endfor %}
      {% unless forloop.last == true %}
      ---
      {% endunless %}
      {% endfor %}
---