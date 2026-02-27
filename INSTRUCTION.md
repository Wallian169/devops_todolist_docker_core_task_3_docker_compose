# 🐳 Django & MySQL Dockerized Project

This project uses **Docker Compose** to orchestrate a Django web application and a MySQL database. It is configured with advanced health checks to ensure seamless startup and automatic database migrations.

---

## 🏗 System Architecture

* **Django App**: The web service, accessible at `http://localhost:8080`.
* **MySQL Database**: A persistent database service using MySQL 9.6.
* **Healthcheck Mechanism**: The Django container waits for the MySQL container to report a `healthy` status before executing migrations and starting the server.
* **Networking**: Both services communicate over a private bridge network named `db-data-net`.
* **Persistence**: A named volume `db_data` ensures your data is saved even if containers are deleted.



---

## Configuration Reference

### Database Settings (`settings.py`)
To allow Django to find the database inside the Docker network, ensure your `DATABASES` dictionary uses the **service name** as the host:

```python
DATABASES = {
    'default': {
        'ENGINE': 'mysql.connector.django', 
        'NAME': 'app_db',
        'USER': 'app_user',
        'PASSWORD': '1234',
        'HOST': 'db-service',  # Matches the service name in docker-compose.yml
        'PORT': '3306',
    }
}

Use these commands for rapid development and troubleshooting.
```

---

## 🛠 Lifecycle Management

| Command | Action |
| :--- | :--- |
| `docker compose up --build` | Recompile images and start containers |
| `docker compose up -d` | Start containers in the background |
| `docker compose stop` | Pause containers (preserves state) |
| `docker compose start` | Resume paused containers |
| `docker compose restart app` | Restart only the Django application |
| `docker compose down` | Stop and remove containers and networks |
| `docker compose down -v` | **Nuclear Option**: Stop and delete ALL data |

---

## 🔍 Debugging & Inspection

| Command | Action |
| :--- | :--- |
| `docker compose ps` | Check container health and port mapping |
| `docker compose logs -f` | Stream logs from all services |
| `docker compose logs -f app` | Stream logs only from Django |
| `docker compose top` | View running processes inside containers |
| `docker compose images` | List images used by the current project |

---

## 🐍 Django Inside Docker

Run these while the containers are active:

### Database & Migrations
* **Make Migrations**: `docker compose exec app python manage.py makemigrations`
* **Apply Migrations**: `docker compose exec app python manage.py migrate`
* **Check DB Status**: `docker compose exec app python manage.py showmigrations`

### User Management
* **Create Admin**: `docker compose exec app python manage.py createsuperuser`
* **Change Password**: `docker compose exec app python manage.py changepassword <username>`

### Maintenance
* **Collect Static**: `docker compose exec app python manage.py collectstatic --no-input`
* **Interactive Shell**: `docker compose exec app python manage.py shell`

---

##  Maintenance

| Command | Action |
| :--- | :--- |
| `docker system prune` | Remove unused data (free up disk space) |
| `docker compose build --no-cache` | Force a fresh build from scratch |