## Overview

Backup or copy all the tables from the database to `csv`  file. 

## Prerequisites

- [psql]()

## Create the PowerShell Script

```powershell
# Database connection details
$DB_NAME = "cerxosdb"
$DB_USER = "cerxos-admin"
$DB_HOST = "159.65.132.241"
$DB_PASS = "f203df5d946b1b25"

# Get the list of tables
$tables_query = "SELECT table_name FROM information_schema.tables WHERE table_schema = 'public';"
$tables_result = psql -h $DB_HOST -U $DB_USER -d $DB_NAME -t -c $tables_query
$tables = $tables_result -split "`n" | ForEach-Object { $_.Trim() } | Where-Object { $_ -ne "" }

# Export each table to a CSV file
foreach ($table in $tables) {
    $csv_file = "${table}.csv"
    $copy_command = "\copy public.`"$table`" TO '$csv_file' CSV HEADER"
    psql -h $DB_HOST -U $DB_USER -d $DB_NAME -c $copy_command
}
```

### Steps to Run the Script

Ensure the [[PostgreSQL]] client tools (including `psql`) are installed on your Windows machine and added to your system's PATH. You can download and install them from the [PostgreSQL website](https://www.postgresql.org/).

Save the script to a file named `ExportTables.ps1`.

Open PowerShell with administrative privileges.

Navigate to the directory where you saved `ExportTables.ps1` and run it:
```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force
.\ExportTables.ps1
```
### Notes

Ensure that can connect to the [[PostgreSQL]] database from your Windows machine. This might involve setting up appropriate network permissions or SSH tunnels if your database is not directly accessible.

If the [[PostgreSQL]] server requires a password for the user, might need to handle this securely. Use a `.pgpass` file to store the password securely or prompt for the password in the script.

The script saves the CSV files in the current working directory. 