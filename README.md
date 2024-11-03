# PostgreSQL-database-Python-Django-REST-API-
PostgreSQL database &amp;  Python/Django REST API on Ubuntu 24 server  with Gunicorn

# PostgreSQL Deployment on Ubuntu 24

This guide outlines the steps to deploy a PostgreSQL database on an Ubuntu 24 server.

1. Update System Packages

```bash
sudo apt update
```

2. Install PostgreSQL

```bash
sudo apt install postgresql -y
```

3. Verify PostgreSQL Service (Optional)

```bash
sudo systemctl status postgresql
```

4. Access PostgreSQL Shell and Configure Database

```bash
sudo -u postgres psql
```

Execute the following SQL commands:

```sql
-- Create the database
CREATE DATABASE recipe;

-- Create the user and set the password
CREATE USER admin WITH ENCRYPTED PASSWORD '12345';

-- Change the database owner to admin
ALTER DATABASE recipe OWNER TO admin;

-- Grant all privileges to the user for the database
GRANT ALL PRIVILEGES ON DATABASE recipe TO admin;

-- List databases (optional)
\l

-- Change password of an existing user (if applicable)
ALTER USER postgres WITH PASSWORD '123456';

-- Exit PostgreSQL shell
\q

```



# Deploy a Python/Django REST API on Ubuntu 24 with Gunicorn

This guide outlines the steps to deploy a Python/Django REST API on an Ubuntu 24 server using Gunicorn.

1. Update System Packages

```bash
sudo apt update
```

2. Install Pip and Python Virtual Environment Package

```bash
sudo apt install python3-pip python3-virtualenv -y
```

3. Create and Activate Virtual Environment

```bash
python3 -m virtualenv venv
source venv/bin/activate
```

4. Clone the Project

```bash
git clone https://github.com/GerromeSieger/RecipeApp-Django.git
cd RecipeApp-Django
```

5. Install Dependencies

```bash
pip install --upgrade setuptools
pip install -r requirements.txt
```

6. Configure Database

Ensure your database connection is properly configured in the Django settings

7. Run Migrations

```bash
python manage.py migrate
```

8. Run Application with Gunicorn

```bash
gunicorn --bind 0.0.0.0:8000 RecipeApp.wsgi:application
```

9. Verify Deployment

Open a web browser and navigate to http://publicip:8000/swagger to access the swagger documentation.

## Alternative Deployment Strategy (Using Systemd)

10. Create Gunicorn Service File

```bash
sudo nano /etc/systemd/system/gunicorn.service
```

Add the following content (adjust paths as necessary):

```ini
[Unit]
Description=Gunicorn daemon for Django API
After=network.target

[Service]
User=root
Group=www-data
WorkingDirectory=/root/RecipeApp-Django
ExecStart=/root/venv/bin/gunicorn --workers 3 --bind 0.0.0.0:8000 RecipeApp.wsgi:application

[Install]
WantedBy=multi-user.target

```
11. Reload Daemon, Start and Enable gunicorn Service

```bash
sudo systemctl daemon-reload
sudo systemctl start gunicorn
sudo systemctl enable gunicorn
sudo systemctl status gunicorn
```
