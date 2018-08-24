---
title: "Stitch Scripts: Reference"
permalink: /stitch-scripts/reference

type: "reference"

sections:
  - content: |
      {% assign categories = "Connections|Connection clients|Packages" | split: "|" %}

      {% for category in categories %}
      {% assign all-reference-docs = site.stitch-scripts | where:"content-type",page.type | sort: title %}

      {% assign category-name = category | downcase | replace:" ","-" %}
      {% assign reference-docs = all-reference-docs | where:"category",category-name %}

      ## {{ category }}

      {% for doc in reference-docs %}
      ### [{{ doc.title | remove: "Stitch Scripts: " }}]({{ doc.url | prepend: site.baseurl }})

      {{ doc.summary | flatify | markdownify }}
      {% endfor %}
      {% unless forloop.last == true %}
      ---
      {% endunless %}
      {% endfor %}
---