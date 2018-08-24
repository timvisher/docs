---
title: "Stitch Scripts: Tutorials"
permalink: /stitch-scripts/tutorials

type: "tutorial"

sections:
  - content: |
      {% assign scripts-tutorial-docs = site.stitch-scripts | where:"content-type",page.type %}

      {% for doc in scripts-tutorial-docs %}
      ### [{{ doc.title | remove: "Stitch Scripts: " }}]({{ doc.url | prepend: site.baseurl }})

      {{ doc.introduction }}
      {% endfor %}
---