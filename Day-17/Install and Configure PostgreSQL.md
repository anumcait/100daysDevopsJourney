# Day 17: Install and Configure PostgreSQL

**Problem Statement**

The Nautilus application development team has shared that they are planning to deploy one newly developed application on Nautilus infra in Stratos DC. The application uses PostgreSQL database, so as a pre-requisite we need to set up PostgreSQL database server as per requirements shared below:

PostgreSQL database server is already installed on the Nautilus database server.

a. Create a database user kodekloud_roy and set its password to BruCStnMT5.

b. Create a database kodekloud_db7 and grant full permissions to user kodekloud_roy on this database.

Note: Please do not try to restart PostgreSQL server service.

---

## 📝 Task Description

The Nautilus application development team plans to deploy a newly developed application that uses PostgreSQL as its backend database. The goal is to prepare the PostgreSQL server by:

1. Creating a new database user
2. Creating a new database
3. Granting full privileges to the user on the database

---

## ✅ Requirements

- PostgreSQL is **already installed** on the target server.
- **Do not restart** the PostgreSQL service.
- Perform all configuration via the PostgreSQL CLI (`psql`).

---

## 🛠️ Steps Performed

### 1. Switch to the `postgres` system user

```bash
sudo -i -u postgres
