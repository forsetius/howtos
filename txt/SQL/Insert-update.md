## Create UUID without any Postgres extension
```sql
SELECT md5(random()::text || clock_timestamp()::text)::uuid;
```

## Cast timestamp to integer
```sql
cast(extract(epoch from current_timestamp + '2 years') as integer)
```

## Temporary tables

### Create from selected data
```sql
CREATE TEMP TABLE temp_users AS
  SELECT * FROM "Users" WHERE "organizationId"=34;
```

### Create using JOIN ON other table for WHERE
```sql
CREATE TEMP TABLE temp_user_auths AS
  SELECT "UserAuths".*
  FROM "UserAuths"
  JOIN "Users" ON "UserAuths"."userId" = "Users"."id"
  WHERE "Users"."organizationId"=34;
```

### Copy multiple records with changes
* Create temporary table
* Update data in temp table
* Insert back into original table

```sql
CREATE TEMP TABLE temp_users
  AS SELECT * FROM "Users" WHERE "organizationId"=34;

UPDATE temp_users SET id = id + 2000, "organizationId"=36;

INSERT INTO "Users" SELECT * FROM temp_users;
```

## Update JSON column

### Set value if key exists
```sql
UPDATE user_rights
SET subject = (case when subject ? 'org'
               then jsonb_set(
                 subject,
                 '{org}'::text[],
                 '36'::jsonb)
               else subject end
              );
```

### Update value if key exists
```sql
UPDATE user_rights
SET subject = (case when subject ? 'user'
               then jsonb_set(
                 subject,
                 '{user}'::text[],
                 (((subject->'user')::integer + 2000)::text)::jsonb)
               else subject end
              );
```

## Copy values from a column in a range of records to another range of records
```sql
update "UserAuths"
set "authId" = sub.naid
from (
  select id as sid, "authId" as naid from "UserAuths"
) as sub
where "id" > 1218 and "UserAuths"."id" = sub.sid + 17;
```
