---
# -------------------------- #
#      Page & Formatting     #
# -------------------------- #

title: DATABASE-INTEGRATION
keywords: database_integration, database integration, etl database_integration, database_integration etl
tags: [database_integrations]
permalink: /integrations/databases/database_integration
summary: "Connect and replicate data from your DATABASE-INTEGRATION database using Stitch's DATABASE-INTEGRATION integration."

# -------------------------- #
#     Integration Details    #
# -------------------------- #

name: "database_integration"
display_name: "DATABASE_INTEGRATION"
author: "Stitch"
status: "Released"
frequency: "30 minutes"
tier: "Free" ## Free vs Paid Stitch plan
port: ## Database's default port - ex: 3306
db-type: "mysql" 	## mysql,postgres,mongo,mssql
icon: /images/integrations/icons/database_integration.svg

# -------------------------- #
#       Stitch Supports      #
# -------------------------- #

versions: "n/a" ## If Stitch only supports certain versions, enter them here
ssh: true	## true if Stitch supports SSH connections
ssl: false	## true if Stitch supports SSL connections
sync-views: false	## true if Stitch supports syncing database views

whitelist: ## indicates ability to select individual tables & columns for replication
  tables: "Yes"		## can the user whitelist tables?
  columns: "Yes"	## can the user whitelist columns?


setup-steps:
  - title: "whitelist stitch ips"

  - title: "retrieve public key"

  - title: "create linux user"

  - title: "create db user"

  - title: "connect stitch"

  - title: "replication frequency"

  - title: "sync data"
---
{% assign integration = page %}
{% include misc/data-files.html %}