# eddb2mysql

Quick scripts to import data dumps from eddb.io into mysql

EDSM is used for the bodies table, all other data including systems and stations comes from EDDB.io

## Requirements

- A running, connectable SQL server (tested on MySQL and MariaDB)
- A database and user with the following permissions
  - SELECT, INSERT, UPDATE, DELETE, CREATE, DROP, and LOCK
  - Ability to execute LOAD DATA INFILE LOCAL, which might require secure mode being off, or executing from a diretory that's allowed to load files. Depending on your setup this might also require FILE permissions
  - RELOAD privileges for the FLUSH command
- wget and gzip to download files
- Node.js is required for json2csv

## New Database Setup

To set up the database, run rebuild.sh. This script downloads the data files from eddb.io and edsm.net and runs the rebuild.sql script to populate the db. For the script to automatically connect to MySQL, put your connection info in mysqlinfo.txt, see mysqlinfo-example.txt for more information.

If you opt to not use mysqlinfo.txt, run rebuild.sh to download the data files, then run the rebuild.sql script against your database manually, e.g:

`mysql -u root -p ed < rebuild.sql`

## Update Existing Database

Recently changed systems and bodies are update once a week on EDDB and EDSM. To update an existing database, run updateweekly.sh. This script update the listings table as well as the bodies and systems tables.

Currently the update script doesn't check to see if, for example, a new allegiance has been formed. In the case of something in the auxiliary tables get added/removed/modified, the easiest recourse is to rebuild the database. Since the DB is pretty small it hopefully isn't that big a deal.

A daily script is included to (optionally) recreate the market listings table on a more frequent basis. Although it's called "updatedaily.sh", it could be run even more frequently if desired. The only caveat to using the daily script is that it probably shouldn't be run at the same time as a rebuild or the weekly update script.

### Example crontab

```
# Update Elite Dangerous database
# Download weekly updates (bodies, systems, listings)
@weekly cd eddb2mysql && updateweekly.sh > /dev/null 2>&1
# Uncomment below to update the listings table every day except when the weekly job runs
#0 0 * * 1-6 cd eddb2mysql && updatedaily.sh > /dev/null 2>&1

# Uncomment below to rebuild entire database every quarter
#0 0 1-7 1,4,7,10 1 cd eddb2mysql && rebuild.sh > /dev/null 2>&1
```

## Contrib

### json2csv (zemirco/json2csv)

https://github.com/zemirco/json2csv

http://zemirco.github.io/json2csv

Included under MIT license, used to process JSON files into CSV for easier import to MySQL
