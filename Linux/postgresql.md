INSTALLING POSTGRESQL
=====================
### Step 1 – Enable PostgreSQL Apt Repository
    apt-get install wget ca-certificates
    wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
##### Now add the repository to your system.
    sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -cs`-pgdg main" >> /etc/apt/sources.list.d/pgdg.list'
### Step 2 – Install PostgreSQL
    apt-get update
    apt-get install postgresql postgresql-contrib
### Step 3 – Connect to PostgreSQL and change password for postgres user
    sudo su - postgres
    psql
    \password
    \conninfo
    \q
### Enable remote connection
#### Edit pg_hba.conf
    # IPv4 local connections:
    host    all             all             0.0.0.0/0               md5
#### Edit postgresql.conf
    # - Connection Settings -
    listen_addresses = '*'          # what IP address(es) to listen on;
