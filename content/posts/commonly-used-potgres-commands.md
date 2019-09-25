---
title: "Commonly Used Potgres Commands"
date: 2019-09-25T11:23:09+03:00
draft: false
toc: false
images:
tags: 
  - untagged
---

## What is psql

`psql` is the interactive terminal for administering postgre. The most important flags available are:

- `h` the host to connect to
- `U` the user to connect with
- `p` the port to connect to (default is 5432)

> psql -h localhost -U username databasename

The other option is to use a string and let psql parse it:

> psql "dbname=dbhere host=hosthere user=userhere password=pwhere port=5432 sslmode=require"

Once connected, you can use `\?` which will give you a list of available commands.

### Meta-commands

Meta-commands are commands that are evaluated by psql and often translated into SQL that is issued against the system tables on the server, saving administrators time when performing routine tasks. Denoted by a backslash `\`

### Turn query timing on

By default the timing of query results will not be available, but we can turn it on by using the following command. This will show query timining in millisecnds.

```bash
# \timing
Timing is on.
```

### List tables in database

```bash
# \d
      List of relations
 Schema |   Name    | Type  | Owner
--------+-----------+-------+-------
 public | employees | table | craig
(1 row)
```

### Describe a table

```bash
# \d employees 
           Table "public.employees"
  Column   |         Type          | Modifiers 
-----------+-----------------------+-----------
 id        | integer               |
 last_name | character varying(50) |
 salary    | integer               |
Indexes:
    "idx_emps" btree (salary)
```

### List all tables in database along with some additional information

```bash
# \d+
       List of relations
Schema | Name  | Type  |   Owner   |  Size  | Description
-------+-------+-------+-----------+--------+-------------
public | users | table | jarvis    | 401 MB |
(1 row)
```

### Describe a table with additional information

```bash
#\d+ users
          Table "public.users"
  Column   |  Type   |   Modifiers   | Storage  | Stats target | Description
-----------+---------+---------------+----------+--------------+-------------
userid     | bigint  | not null      | plain    |              |
fullname   | text    | not null      | extended |              |
email      | text    | not null      | extended |              |
phone      | text    | not null      | extended |              |
credits    | money   | default 0.0   | plain    |              |
parked     | boolean | default false | plain    |              |
terminated | boolean | default false | plain    |              |

Indexes:
    "users_pkey" PRIMARY KEY, btree (userid)
```

### List all databases

```bash
# \l
          List of databases
Name     |   Owner   | Encoding | Collate | Ctype |      Access privileges
---------+-----------+----------+---------+-------+-----------------------------
learning | jarvis    | UTF8     | C       | UTF-8 |
```

### List all databases with additional information

```bash
# \l+
          List of databases
  Name    |   Owner   | Encoding | Collate | Ctype |      Access privileges      |  Size   | Tablespace |                Description
----------+-----------+----------+---------+-------+-----------------------------+---------+------------+--------------------------------------------
learning  | jarvis    | UTF8     | C       | UTF-8 |                             | 492 MB  | pg_default |
```

### List all schemas

```bash
# \dn
List of schemas
 Name  | Owner
-------+--------
public | jarvis
(1 row)
```

### List all schemas with additional information

```bash
# \dn+
List of schemas
Name   | Owner  | Access privileges |      Description
-------+--------+-------------------+------------------------
public | jarvis | jarvis=UC/jarvis +| standard public schema
       |        | =UC/jarvis        |
(1 row)
```

### List all functions

```bash
#\df
```

### List all functions with additional information

```bash
#\df+
```

### Connect to another database

```bash
#\c dbname
```

### Quit from postgres shell

```bash
#\q
```

### Text editor inside psql

```bash
#\e
```

This opens your default text editor inside psql shell.Pretty handy for query modifications.

The commands can be given a regex for eg. \df *to_array* lists all functions that contain to_array in its name.

Of course there are lot many other commands, as said above `\?` will list all of them, and from there we can pick up whatever we want.

#### Conclusion

Psql is a powerful tool once we master it, and since it is command line, we can use it across environments.
