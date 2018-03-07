---
# -------------------------- #
#      Page & Formatting     #
# -------------------------- #

title: Oracle
keywords: oracle, database integration, etl oracle, oracle etl
tags: [oracles]
permalink: /integrations/databases/oracle
summary: "Connect and replicate data from your Oracle database using Stitch's Oracle integration."

# -------------------------- #
#     Integration Details    #
# -------------------------- #

name: "oracle"
display_name: "Oracle"
author: "Stitch"
status: "Closed Beta"
frequency: "30 minutes"
tier: "Enterprise"
port: 1521
db-type: "oracle"
icon: /images/integrations/icons/oracle.svg

# -------------------------- #
#       Stitch Supports      #
# -------------------------- #

certified: true

versions: "n/a"
ssh: true
ssl: true
sync-views: false

whitelist:
  tables: true
  columns: true

data-types:
  - name: "integer"
    supported: true
    maps-to: &integer "integer"

  - name: "number(*,0)"
    supported: true
    maps-to: *integer

  - name: "number(*,*)"
    supported: true
    maps-to: &decimal "decimal"

  - name: "int"
    supported: true 
    maps-to: *integer

  - name: "smallint"
    supported: true
    maps-to: *integer

  - name: "float"
    supported: true
    maps-to: *decimal

  - name: "double_precision"
    supported: true 
    maps-to: *decimal

  - name: "real"
    supported: true
    maps-to: *decimal

  - name: "binary_float"
    supported: true
    maps-to: &float "float"

  - name: "binary_double"
    supported: true
    maps-to: *float

  - name: "date"
    supported: true
    maps-to: &timestamp "timestamp"

  - name: "timestamp with time zone"
    supported: true
    maps-to: *timestamp

  - name: "timestamp with local time zone"
    supported: true
    maps-to: *timestamp

  - name: "char(n byte)"
    supported: true
    maps-to: &string-no-max "string (no max length)"

  - name: "char(n char)"
    supported: true
    maps-to: &string-max "string (max length n chars)"

  - name: "nchar(n char)"
    supported: true
    maps-to: *string-max

  - name: "nvarchar2"
    supported: true
    maps-to: *string-max

  - name: "varchar(n byte)"
    supported: true
    maps-to: *string-no-max

  - name: "varchar(n char)"
    supported: true
    maps-to: *string-max

  - name: "long"
    supported: false

  - name: "long raw"
    supported: false

  - name: "clob"
    supported: false

  - name: "blob"
    supported: false

  - name: "nclob"
    supported: false

  - name: "adt"
    supported: false

  - name: "collections"
    supported: false

  - name: "time intervals"
    supported: false

  - name: "bfile"
    supported: false

  - name: "ref"
    supported: false 

# -------------------------- #
#     Setup Instructions     #
# -------------------------- #

requirements-list:
  - item: "Require access to the source (`v$database`)"
  - item: "**Create database user permissions**."
  - item: "**Access to LogMiner**. (Optional)"

setup-steps:
  - title: "whitelist stitch ips"

  - title: "Retrieve the Database's Oracle System ID"
    anchor: "retrieve-oracle-system-id"
    content: |
      An Oracle System ID (SID) is used to uniquely identify a specific database in your system. When you connect an {{ integration.display_name }} database to Stitch, you'll enter the SID of the database you want Stitch to extract data from.

      [Instructions for how to do this?]

  - title: ""
    anchor: ""
    content: |
      

  - title: "Enable Incremental Replication with LogMiner"
    anchor: "enable-logminer"
    content: |
      {% include note.html content="Some data types may not be supported for Incremental Replication. Refer to the [Data Type Mapping](#data-type-mapping) section for more info." %}

      Incremental Replication is the most efficient way to replicate {{ integration.display_name }} data. Stitch uses Oracle's LogMiner functionality to query Oracle's archive logs and retrieve all inserts, updates, and deletes to your database.

      To use log-based replication, your database must be configured in the following manner:

      1. **ARCHIVELOG mode must be enabled**. [explanation]

         To check the database's current mode, run:

         ```sql
         SELECT log_mode FROM v$database
         ```

         If the result is `NOARCHIVELOG`, then `ARCHIVELOG` mode must be enabled. Refer to the [Changing the Database Archiving Mode section](https://docs.oracle.com/database/121/ADMIN/archredo.htm#ADMIN-GUID-C12EA833-4717-430A-8919-5AEA747087B9) in Oracle's documentation for instructions on how to do this.

      2. **Supplemental logging must be enabled.** [explanation]

         To enable supplemental logging, run:

         ```sql
         ALTER DATABASE ADD SUPPLEMENTAL LOG DATA (???) COLUMNS;
         ```

      3. **Stitch's database user must have permissions to run LogMiner**. [explanation]

         ```sql
         [PLACEHOLDER] WHAT PERMISSIONS DO WE NEED????
         ```

  - title: "connect stitch"

  - title: "Define the Default Replication Method"
    anchor: "define-default-replication-method"
    content: |
      Next, you'll select the Replication Method that tables will use by default. This is the method that each table will automatically use once selected for replication, but you can always override it if necessary.

      <table width="100%; fixed">
          <tr>
              <td width="50%; fixed">
                  <strong>Log-based Incremental Replication</strong>
              </td>
              <td>
                  <strong>Full Table Replication</strong>
              </td>
          </tr>
          <tr>
              <td>
                  <ul>
                      <li>The most efficient way to replicate data: Less overall row usage and lower latency</li>
                      <li>Only replicates new and updated records upon each replication attempt: Uses <a href="https://docs.oracle.com/en/database/oracle/oracle-database/18/strms/strms_glossary.html#STRMS-GUID-3751F0F8-7851-4C5A-83EC-7652881948D7">Commit System Change Numbers (CSCN)</a> to select new and updated data for replication</li>
                      <li>Identifies hard-deleted records using a Stitch system column - see the <a href="#deleted-records">Deleted Records</a> section</li>
                      <li>Some data types may not be supported - see the <a href="#data-type-mapping">Data Type Mapping</a> section for details</li>
                      <li>Utilizes Oracle's LogMiner feature, which requires some <a href="#enable-logminer">additional configuration</a></li>
                  </ul>
              </td>
              <td>
                  <ul>
                      <li>The least efficient way to replicate data: More overall row usage and may increase latency</li>
                      <li>Replicates a table's contents fully upon each replication attempt</li>
                      <li>Captures hard-deleted records</li>
                      <li>[PLACEHOLDER - DATA TYPE SUPPORT?]</li>
                      <li>Doesn't require any additional configuration</li>
                  </ul>
              </td>
          </tr>
      </table>

  - title: "historical data"
  ## default is 1 year

  - title: "replication frequency"

  - title: "sync data"

replication-sections:
  - title: "Data Type Mapping"
    anchor: "data-type-mapping"
    content: |
      {% include misc/support-icons.html %}

      While LogMiner supports a variety of data types, there are some that Stitch will be unable to replicate due to a lack of Oracle support. In the table below are the data types that can and can't be replicated by Stitch.

      - **Supported data types** are marked with an {{ supported | replace: "TOOLTIP","Supported data type" }} icon and include what the data type will map to in the destination
      - **Unsupported data types** are marked with an {{ not-supported | replace: "TOOLTIP","Unsupported data type" }} icon. According to [Oracle's documentation](https://docs.oracle.com/cloud/latest/db112/SUTIL/logminer.htm#SUTIL1620), tables containing any unsupported data types will be ignored by LogMiner.

      {% assign data-types = integration.data-types | sort:"name" %}
      <table width="100%; fixed">
          <tr>
              <td align="right">
                  <strong>Data Type</strong>
              </td>
              <td>
                  <strong>Mapping</strong>
              </td>
              <td>
                  <strong>Supported</strong>
              </td>
          </tr>
          {% for data-type in data-types %}
              <tr>
                  <td align="right">
                      <code>{{ data-type.name | upcase }}</code>
                  </td>
                  <td>
                      <code>{{ data-type.maps-to | upcase }}</code>
                  </td>
                  <td align="center">
                      {% case data-type.supported %}
                          {% when true %}
                              {{ supported | replace: "TOOLTIP","Supported data type" }}
                          {% else %}
                              {{ not-supported | replace: "TOOLTIP","Unsupported data type" }}
                      {% endcase %}
                  </td>
              </tr>
          {% endfor %}
      </table>
---
{% assign integration = page %}
{% include misc/data-files.html %}