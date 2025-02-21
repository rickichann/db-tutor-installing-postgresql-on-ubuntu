# Install PostgreSQL on EC2 Ubuntu

## Step 1: Connect to EC2 Instance

Connect to EC2 Instance Using AWS Systems Manager

## Step 2: Update System Packages
Update the package list:
```sh
sudo apt update && sudo apt upgrade -y
```

## Step 3: Add PostgreSQL Repository
Add the official PostgreSQL repository:
```sh
sudo apt install -y wget
wget -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo tee /etc/apt/trusted.gpg.d/postgresql.asc
echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" | sudo tee /etc/apt/sources.list.d/pgdg.list
sudo apt update
```

## Step 4: Install PostgreSQL
Install the latest PostgreSQL version:
```sh
sudo apt install -y postgresql
```
Check the installed version:
```sh
psql --version
```

## Step 5: Start and Enable PostgreSQL
Ensure PostgreSQL is running:
```sh
sudo systemctl start postgresql
sudo systemctl enable postgresql
```
Check PostgreSQL status:
```sh
sudo systemctl status postgresql
```

## Step 6: Configure PostgreSQL
### Change Default Password for `postgres` User
```sh
sudo -i -u postgres
psql
```
Run the following command in the PostgreSQL CLI:
```sql
ALTER USER postgres PASSWORD 'your-secure-password';
```
Exit PostgreSQL:
```sql
\q
exit
```

## Step 7: Allow External Connections (Optional)
### Modify PostgreSQL Configuration
Edit the PostgreSQL configuration file:
```sh
sudo nano /etc/postgresql/*/main/postgresql.conf
```
Find `listen_addresses` and change it to:
```conf
listen_addresses = '*'
```

Edit `pg_hba.conf` to allow external connections:
```sh
sudo nano /etc/postgresql/*/main/pg_hba.conf
```
Add the following line at the bottom:
```conf
host    all             all             0.0.0.0/0               md5
```
Restart PostgreSQL:
```sh
sudo systemctl restart postgresql
```

### Configure Security Group in AWS
1. Open AWS Console > EC2 > Security Groups.
2. Edit the Security Group associated with your EC2 instance.
3. Add an inbound rule:
   - **Protocol**: TCP
   - **Port Range**: 5432
   - **Source**: Your IP or `0.0.0.0/0` (for public access, not recommended)

## Step 8: Verify Connection
Try connecting from your local machine:
```sh
psql -h your-ec2-ip -U postgres -d postgres
```

## Conclusion
You have successfully installed PostgreSQL on an EC2 Ubuntu instance! 🚀 If you need further assistance, feel free to ask.

