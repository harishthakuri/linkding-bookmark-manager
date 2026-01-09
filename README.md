# linkding bookmark manager setup with PostgreSql

[linkding Installation](https://linkding.link/installation/)

## Using docker-compose

### Docker Compose Breakdown

Container Configuration:

- Uses the sissbruecker/linkding:latest image
- Exposes port 9090 (configurable via LD_HOST_PORT)
- Mounts a local data directory to /etc/linkding/data for persistent storage
- Loads environment variables from .env file
- Automatically restarts unless manually stopped

#### How to Use PostgreSQL Database

The .env file does have PostgreSQL configuration! Look at the bottom section labeled "Database settings". Here's what you need to configure:

#### Require files to run docker-compose

- docker-compose.yaml
- .env

```bash

docker compose up -d

# or, if docker-compose is installed
docker-compose up -d


```

To complete the setup, you still have to create an initial user, so that you can access your installation.

**Important notes:**

- By default, Linkding uses SQLite (no database configuration needed)
- When you set LD_DB_ENGINE=postgres, it switches to PostgreSQL mode
- Make sure your PostgreSQL database exists before starting the container
- The database user needs appropriate permissions to create tables

## Using docker run command directly

When using docker run, we add database configuration with the -e flag for each environment variable. Here's how to add PostgreSQL info:

```bash
docker run --name linkding \
  -p 9090:9090 \
  -v {host-data-folder}:/etc/linkding/data \
  -e LD_DB_ENGINE=postgres \
  -e LD_DB_DATABASE=linkding \
  -e LD_DB_USER=your_username \
  -e LD_DB_PASSWORD=your_password \
  -e LD_DB_HOST=your_postgres_host \
  -e LD_DB_PORT=5432 \
  -d sissbruecker/linkding:latest
```

### Comparison: Docker Run vs Docker Compose

Docker Run approach:

- Each environment variable needs its own -e flag
- Can get messy with many variables

Docker Compose approach:

- All environment variables in the .env file
- Cleaner and easier to manage
- Just edit the .env file and run docker-compose up -d

docker-compose setup is actually better because the env_file: - .env line automatically loads all those environment variables from .env file. We just need to uncomment and fill in the database settings at the bottom of your .env file:

```bash
LD_DB_ENGINE=postgres
LD_DB_DATABASE=linkding
LD_DB_USER=myuser
LD_DB_PASSWORD=mypassword
LD_DB_HOST=192.168.1.10
LD_DB_PORT=5432
```

Then run `docker compose up -d` or `docker-compose up -d` and it will connect to PostgreSQL database!

## Access linkding UI

http://<ip_address>:9090/

- user: admin # we configured this in .env file
- password: <your_password> # we configured this in .env file
