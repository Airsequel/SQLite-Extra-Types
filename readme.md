# SQLite Extra Types

Specification on how to extend SQLite with extra datatypes
and a collection of type suggestions.

> ⚠️ This document is still a work in progress


## Motivation

SQLite is a great database, but it is missing some datatypes
that are very useful in many applications.
This document describes how to extend SQLite with extra datatypes
and provides a collection of type suggestions.


## How to extend SQLite

Internally SQLite assigns a type affinity to each column.
The type affinity of a column is the recommended type
for data stored in that column.
Following type affinities are defined:

- `TEXT`
- `NUMERIC`
- `INTEGER`
- `REAL`
- `BLOB`

Those type affinities are assigned according to the following rules
(see [sqlite.org/datatype3](https://www.sqlite.org/datatype3.html)):

1. If the declared type contains the string `"INT"`
    then it is assigned `INTEGER` affinity.
1. If the declared type of the column contains any of the strings
    `"CHAR"`, `"CLOB"`, or `"TEXT"` then that column has `TEXT` affinity.
1. If the declared type for a column contains the string `"BLOB"`
    or if no type is specified then the column has affinity `BLOB`.
1. If the declared type for a column contains any of the strings
    `"REAL"`, `"FLOA"`, or `"DOUB"` then the column has `REAL` affinity.
1. Otherwise, the affinity is `NUMERIC`.

This means:
**We can simply define arbitray types in SQLite
as long as the type affinity algorithm yields the correct storage type.**

We suggest following naming convention for types:

- `TEXT_` for text types
- `INT_` for integer types
- `REAL_` for real types
- `BLOB_` for blob types

And here is the full list of types we suggest
(and which we support on [airsequel.com](https://airsequel.com)):

Name | Description
---- | -----------
`TEXT_DATE` | Date in ISO format `YYYY-MM-DD`
`TEXT_DATETIME` | Date and time in ISO format `YYYY-MM-DD(T)HH:MM:SS.ZZZ(Z)`
`TEXT_TIME` | Time in ISO format `HH:MM:SS.ZZZ` (with optional seconds and milliseconds)
`TEXT_DURATION` | Duration in time format `HH:MM:SS.ZZZ` (with optional seconds and milliseconds)
`TEXT_DURATION_ISO` | Duration in ISO format `P1Y2M3DT4H5M6S`
`TEXT_URL` | URL (with optional protocol)
`TEXT_EMAIL` | Email address
`TEXT_TEL` | Telephone number (only country code and number)
`TEXT_LOCATION` | Location in ISO format `(long, lat)`
`TEXT_COUNTRY` | Country short name in English according to ISO 3166
`TEXT_COUNTRY_ALPHA_2` | Country in ISO 3166-1 alpha-2 format `DE`
`TEXT_COUNTRY_ALPHA_3` | Country in ISO 3166-1 alpha-3 format `DEU`
`TEXT_CODE` | Code in any programming language
`TEXT_CODE_X` | Code in programming language "X"
`TEXT_COLOR` | Color in any color format
`TEXT_COLOR_HEX` | Color in hex RGB format `#ffaa00`
`TEXT_COLOR_RGB` | Color in RGB format `rgb(255,170,0)`
`TEXT_COLOR_X` | Color in format "X"
`TEXT_JSON` | JSON data (string, array, object, number, boolean, or `null`)
`TEXT_JSON_OBJECT` | JSON object
`TEXT_JSON_ARRAY` | JSON array
`TEXT_IBAN` | International Bank Account Number
`TEXT_BIC` | Bank Identifier Code
`TEXT_ISBN` | International Standard Book Number with 13 digits
`TEXT_EAN` | International Article Number with 13 digits
`TEXT_X` | Text in format "X" where "X" is a well defined and common format
====================|====================
`INT_STARS` | Number of stars from 1 to 5
`INT_STARS_10` | Number of stars from 1 to 10
`INT_STARS_100` | Number of stars from 1 to 100
`INT_PERCENT` | Percentage from 0 to 100
====================|====================
`REAL_MONEY` | Money in any currency
`REAL_MONEY_EUR` | Money in Euro
`REAL_MONEY_USD` | Money in US Dollar
`REAL_MONEY_X` | Money in currency "X"
`REAL_PERCENT` | Percentage from 0.0 to 1.0
====================|====================
`BLOB_IMAGE` | Image in any image format
`BLOB_IMAGE_JPEG` | Image in JPEG format
`BLOB_IMAGE_PNG` | Image in PNG format
`BLOB_IMAGE_X` | Image in format "X"
`BLOB_FILE` | File in any format
`BLOB_FILE_PDF` | File in PDF format
`BLOB_FILE_X` | Binary file in format "X"
`BLOB_BIT_N` | Bitfield with length "N"


## TODO

- [ ] `TEXT_INTERVAL`
- [ ] Add types for geographic data structures
      (https://www.sqlite.org/geopoly.html)
- [ ] Postgres types
      (https://tableplus.com/blog/2018/06/postgresql-data-types.html)
- [ ] MySQL types
      (https://www.mysqltutorial.org/mysql-data-types.aspx)
- [ ] SQL Server types
      (https://www.sqlservertutorial.net/sql-server-basics/sql-server-data-types/)
- [ ] Oracle types
- [ ] MariaDB types
      (https://mariadb.com/kb/en/library/data-types/)
- [ ] MongoDB types
      (https://docs.mongodb.com/manual/reference/bson-types/)
