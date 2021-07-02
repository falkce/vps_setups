# Create a PostgreSQL Database on Ubuntu VPS
## Creating the Database
SSH into running server:
```bash
ssh server_user@server_ip
```

Update server packages:
```bash
sudo apt-get update
```

Install Postgres:
```bash
sudo apt-get install postgresql postgresql-contrib
```

Switch to user to 'postgres' in order to be able to execute Postgres commands:
```bash
su - postgres
```

Create a new user that will be used to access the database remotely. In this example we make the new user as superuser so the user will have all priviliges.
```bash
createuser --interactive --pwprompt
```

```bash
Enter name of role to add: test_user
Enter password for new role:
Enter it again:
Shall the new role be a superuser? (y/n) y
```

Next we create the databse and assign the created user to it:
```bash
createdb -O test_user test_database
```

## Configure Remote Access
Open the postgresql configuration file with the editor of your choice (we use nano in this example):
```bash
nano /etc/postgresql/10/main/postgresql.conf
```
and change `#listen_addresses = 'localhost'` to `listen_addresses = '*'`

Next we open the pg_hba configuration file in order to allow access for everyone. You can also limit this to your specific IP.
```bash
nano /etc/postgresql/10/main/pg_hba.conf
```

and change
```bash
# IPv4 local connections:
host    all             all             127.0.0.1/32            md5
```
to
```bash
# IPv4 local connections:
host    all             all             0.0.0.0/0            md5
```

Now open port 5432:
```bash
sudo ufw allow 5432/tcp
```

Finally we restart postgres to apply all changes:
```bash
sudo systemctl restart postgresql
```

You are now able to access your database remotely. Please note that you might have to open port 5432 in the firewall settings of your VPS provider as well.