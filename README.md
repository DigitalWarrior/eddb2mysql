# eddb2mysql

Quick scripts to import data dumps from eddb.io into mysql

## Requirements

- A running, connectable SQL server (tested on MySQL and MariaDB)
  - Must have a database with proper permissions and the ability to execute LOAD DATA INFILE LOCAL
- Node.js is required for json2csv

## Usage

Currently the update script doesn't check to see if, for example, a new allegiance has been formed.  In the case of something in the auxiliary tables get added/removed/modified, the easiest recourse is to rebuild the database.  Since the DB is pretty small it hopefully isn't that big a deal.

### New Database

1. Download the data dump files, you can either run getall.sh or browse to https://eddb.io/api and manually download factions.csv, listings.csv and systems.csv
2. Run the rebuild.sql script against the database to use, e.g:

`mysql -u root -p ed < rebuild.sql`

### Update Existing Database

1. Run getupdate.sh or download systems_recently.csv from: https://eddb.io/api
2. Run update.sql against the existing database

systems_recently.csv gets generated once a week on eddb.io

## Contrib
### json2csv (zemirco/json2csv)

https://github.com/zemirco/json2csv

http://zemirco.github.io/json2csv

Included under MIT license, used to process JSON files into CSV for easier import to MySQL
