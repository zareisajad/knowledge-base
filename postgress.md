
```
sudo pacman -S postgresql
```

Initialize the database cluster (this creates the internal system database):
```
sudo -iu postgres initdb --locale=en_US.UTF-8 -D /var/lib/postgres/data
```

```
sudo systemctl enable --now postgresql
```

Now switch into the PostgreSQL admin account:  
```
sudo -iu postgres
```

Open the PostgreSQL shell:  
```
psql
```

```
CREATE USER myuser WITH PASSWORD 'password';
```

Give this user full privileges:
```
ALTER USER myuser WITH SUPERUSER;

OR 

ALTER USER myuser CREATEDB CREATEROLE;
```

```
CREATE DATABASE mydb OWNER myuser;
```

```
GRANT ALL PRIVILEGES ON DATABASE mydb TO myuser;
```

login to db:

```
psql -U myuser -d mydb
```

- `-d postgres` connects to the default `postgres` database so you can drop your dev DB.
```bash
psql -U your_db_user -d postgres
```


```sql
DROP DATABASE cocode;
CREATE DATABASE cocode;
```