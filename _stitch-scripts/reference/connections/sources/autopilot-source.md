---
title: "Stitch Scripts: AutoPilot Connection"
permalink: /stitch-scripts/reference/connections/autopilot
layout: scripts
sidebar: scripts

content-type: "reference"

short-name: "autopilot"
display-name: "AutoPilot"
type: "source"

resources:
  - title: "{{ connection.display-name }} API Documentation"
    url: "https://autopilot.docs.apiary.io/#reference/"
    description: ""

  - title: ""
    url: ""
    description: ""

description: |
  Use the {{ connection.display-name }} client to update your email contacts, lists, and segments. 

  The Scripts {{ connection.display-name }} client lightly wraps the {{ connection.display-name }} API by auto-generating an authorization header with your API key and translating relative paths in the API to URLs.

query: |
  {{ connection.short-name }}.get("/lists")
---
{% assign connection = page %}