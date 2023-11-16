# PostgreSQL - Postgres SQL Database

## Install Steps

```sh
sudo pacman -S postgresql
```

## Open Shell using `postgres` user

```sh
sudo -iu postgres
```

!!! note
    Your home directory changes to `/var/lib/postgres`. So **Don't do any thing there**

## `postgres` Command Prompt

```sh
psql -U postgres
```

## Set Default Location of DB - Initial Setup

```sh
#1. Create Directory
sudo mkdir -p /pgroot/data
sudo chown -R postgres:postgres /pgroot
```
```sh
#6. Edit the postgresql.service file
sudo vim /usr/lib/systemd/system/postgresql.service
```

```
#6.a Edit Lines Lines
[Service]
Environment=PGROOT=/pgroot
PIDFile=/pgroot/data/postmaster.pid
```

```sh
#2. Enter Shell
sudo -iu postgres
```

```sh
#3. Configure - Using System Default
# - Also need for First time Init
initdb --locale=en_US.UTF-8 -E UTF8 \
  -D /pgroot/data
#4. Exit Shell
exit
```

```sh
#5. For System wide auto-start
sudo systemctl enable --now postgresql
```

## GUI - BeeKeeper Studio

<https://www.beekeeperstudio.io/>

## Change the Table Column Properties

```sql
ALTER TABLE person ALTER COLUMN name SET NOT NULL;
```

## Faster Join Query using `With`

```sql
explain WITH x AS
(
  SELECT gender, count(*) AS res
  FROM t_person AS a
  GROUP By 1
)
SELECT name, res
FROM x, t_gender AS y
WHERE x.gender = y.id
```

There are 2 tables `t_person` and `t_gender`. The `t_person` has 3 fields (`id, name, gender`) and `t_gender` has 2 fields (`id, gender`).
The fist part `with` does aggregation from `t_person` and
then performs the `lookup` operation.

### Reference

Video(Performance Optimization for PostgreSQL)
<https://www.youtube.com/watch?v=5M2FFbVeLSs>

## Foreign Key Constrains

- They do not automatically create Index, need to Manually do that
- Creating a index for the Foreign key helps with UPDATE and DELETE operations of the Reference table
- Without indexes for Foreign Key, the whole table is scanned row by row to remove or edit references of the item UPDATEd or DELETEd from the reference table.
-
### Reference

Video (Deep dive in Indexes)
<https://www.youtube.com/watch?v=HAn1xu6_SW0>

## Check Indexes not being used

```sql
SELECT * FROM pg_stat_user_indexes
```

- Look at `idx_scan`, `idx_tup_read` and `idx_tup_fetch`.

## `explain` `analyze` - Figure out How much time it takes

- <https://www.postgresql.org/docs/13/sql-explain.html>
- Add `explain analyze` before query
- It shows the types of scans and how much time used (`analyze`)
- Types of scan - `Sequential Scan`, `Index Scan`, `Bitmap Index Scan`, `Index only scan`, `Bitmap Heap scan`



----
<!-- Footer Begins Here -->
## Links

- [Back to Linux Hub](./README.md)
- [Back to Root Document](../README.md)


