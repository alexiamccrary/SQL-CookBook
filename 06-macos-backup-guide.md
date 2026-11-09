# MySQL Database Backup and Export Guide (macOS / Linux)

This guide covers how to export (dump) a MySQL database to a `.sql` file using the command-line interface on macOS or Linux.

## Prerequisites
* MySQL installed on your system.
* Terminal access.
* Root (or user) credentials for your MySQL server.

---

## Step 1: Open Terminal
Press `Command + Spacebar` to open Spotlight Search, type `Terminal`, and hit **Enter**.

---

## Step 2: Locate MySQL Binaries (If Necessary)
On macOS, MySQL CLI tools are typically located in `/usr/local/mysql/bin/`. You can reference the full path directly when running commands if `mysqldump` is not globally added to your system `PATH`.

---

## Step 3: Run the `mysqldump` Command
Execute the following command in Terminal, replacing the database name and file path with your own parameters:

```bash
/usr/local/mysql/bin/mysqldump -u root -p your_database_name > /Users/yourusername/Desktop/backup_file.sql
