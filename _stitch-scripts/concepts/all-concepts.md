---
title: "Stitch Scripts: Concepts"
permalink: /stitch-scripts/concepts

type: "concept"

sections:
  - content: |
      {% assign scripts-concept-docs = site.stitch-scripts | where:"content-type",page.type %}

      {% for doc in scripts-concept-docs %}
      ### [{{ doc.title | remove: "Stitch Scripts: " }}]({{ doc.url | prepend: site.baseurl }})

      {{ doc.introduction }}
      {% endfor %}
---